Mobile apps can be HTML 5 based, which do not need a app kind of thingy for the phone, or be totally dependent on the phone Os to work. 

| Andriod | iOS |
| --- | --- |
| Java | Object C |
| No Need to worry about buffer overflows etc. | Need to worry |
| Linux | Unix | 
| Made sandboxing by treating every application as a new user. Android made sandboxing by treating every application as a new user. How to share control or data from one app to another? Using DAC(directory access control) you give access to some users or you can also make it works readable(this can be dangerous because now malicious apps can read everything being written) | Makes use of MAC :Manadatory Access control here OS make decisions on which application can read which data.  | 
| Different apps can read all communication. They talk with each other using ‘intents’ | Apps communicate via URL. And processes can register themselves to certain URLs to be able to process data coming to any URL. | 
| GCA. Makes use of different java libs to do things like encryption, keychain | In house libs|
| Open market place, anyone can run an AppStore for any groups of applications. | Only one AppStore made by Apple people. Apple has to sign the application in order for an app to come on AppStore and then to get installed on a phone | 
| Data leakage a bit easier | | 
| Web Vulns XSS, SQL Injection  | Buffer Overflow, Format String  | 

* Both make use of sandboxing to enable one app doesn’t interfere with another. 
* Both support DEP : Data Execution Protection 
* Both support ASLR : Address space layer randomization (to prevent XSS attack)

__Google bouncer__
Vetting process for android applications to be posted on Google Play. 
_Dynamic Analysis_ monitors network application, will also look at what all application is trying to do. 
Bouncer is run on Linux, allows apps to have network access while testing it. A great emulation on the env. 
Bouncer runs for 5 mins to test it. Doesn’t have a text identification on it. Emulates Android Env. Emulates inputs clicks 

__Guidelines__
* While writing the server side code for the mobile app, validate the connection before sharing data. 
* Encrypt whatever and wherever needed. People could be listening to the msgs between server and mobile App. 
* People can reverse Engineer the app, so it should have secure way to generate the keys. 
* M1: Weak Server Side Controls - Authenticate connection 
* M2: Insecure Data Storage - Do not assume only client app will connect to the server 
* M3: Insufficient Transport Layer Protection - Encrypt 
* M4: Unintended Data Leakage : Do not make data public on the phone, do not get data which is not needed 
* M5: Poor Authorization and Authentication
* M6: Broken Cryptography
* M7: Client Side Injection -> check what data is being sent from the server before executing it blindly
* M8: Security Decisions Via Untrusted Inputs
* M9: Improper Session Handling
* M10: Lack of Binary Protections : try and protect your binary to ensure people are not able to reverse engineer your app. 


## UNIX Security review 
* __Principle of Least privilege__ ability to access or modify a resource . A system module should only have the privileges it
needs to be able to do the thing it wants to, so as to be able to limit the damage that the code can do.
* Unix access control 
__access control lists__ control which user can access which files. So its a store that associates a user to a file. 
Unix also allows to group users. And that can make it easier for people to grant access to files etc. 
* Android process isolation : Each android process runs with a new user id. Provides memory protection between apps. 
Communication is restricted to Unix domain sockets. 
Zygot : only process which runs are root. This process is responsible for starting the rest of the apps. 


__Process user id__ each process has 3 id, which it inherits from the process that started it.  
* * * _Real User id_ => same as user ID of parent process => used to determine which user started the process 
* * * _Effective user id_ =>  from set user ID bit on the file being executed, or sys call -> determine the permissions from process, -> file access and port binding 
* * * _Saved user id_ last used user id for a process. -> so previous EUID can be restored. 
Fork and Exec : inherit the 3 id’s ; except in certain case when the exec of file with setUid bit. In these cases it can toggle back and forth between current ans saved user id. 

__SetUid system call__ the id of process created by executing this file is set to the Id of the file owner. 
This allows the program running under this id to have all the privileges as that of its owner 

__Managed code__ (like JVM) Bytecode execution env : 
* Verifier checks byte code is working in correct form. Bytecode that’s generating might or might not be coming from 
verified java compiler. That’s why we have these verifiers. To detect anything malicious. 
Every instruction is well formed. 
* Run time checks every instruction is well formed. If there’s a branch it should be something in the code. Method 
call should be to a method which is defined. And type checks
* Memory safety 
* Compiler has built in type safety
Compiler generates bytecode which can be run in a virtual machine. loads classes on demand. Execution of the code can be 
interpreted. Machine dependent part of the code gets minimized. Checks can be done in easier way if run in this virtual 
env rather than directly on the machine. Always working on compiled source code. 
* __Linker__ adds compiled class to runtime system. Creates static fields and initialises them. Resolves symbolic names to 
direct references. 
* __Bytecode interpreter__ Perform runtime checks such as array bounds, 
It’s possible to compile byte class to native class 
All casts are checked to be type safe. All pointers references are checked for memory leaks etc. garbage collector 
Prevents : Buffer overflows. 
* __Permission checking__ want to make sure in runtime that only processes who can read/write to a file are the ones who have 
access to it. 
* __Stack inspection__ its possible that method f calls g which calls h and then h access the files. We would need to make 
sure that all three h, g, and f have access to the file, because either of g or f could be malicious and calling h to 
access file h which they know has access to the file. 
* Confused deputy problem: to ensure that no method should be able to do something malicious on behalf of someone. 
where an App 1 does not have an access to Thing A, but app 2 has. App 1 is able to manipulate App2 into doing thing A. 
If the app 2 doesn’t have enough permissions check, it can very easily become vulnerable to this attack. 

### Application sandbox
* Each application runs with its UID in Its own runtime environment 
* *  Provides CPU protection, memory protection
* *  Only zygote (spawn another process) run as root ( Create a whitelist model)
* User grants access at install time
It possible for apps to request for custom permissions. (perfect attack : rename these customs permissions harmless but do something more scary at the background ??)
Otherwise : only allow permissions to a given app is user has allowed them 
* Communication between applications
* *  May share same Linux user ID
* *  Access files from each other
* *  May share same Linux process and runtime environment
If a application wants to control some component in other layer or other application ? 
The component will have a permission requirement listed on it. And if the accessing application has the needed permissions 
it will be able to access the component else not. 
* *  Or communicate through application framework
* *  __Intents__ reference monitor checks permissions __Asyn messaging system__
Each process has a listener for intent. And each action generates an intent 
Intent is a bundle of info. Contains some additional data : what's the action, data to act on the action, component, instructions on how to act ? 
intent can be for a specific process, or can be generic and then every process which has a listener for that activity will get the intent 
If sender wants only certain apps with certain control receive the sent intent, then it can post it with its permissions access and process with that access or larger only will be able to access it. 
=> receiver needs to perform input checking before actually acting on the given intent. 
* Dlmalloc: Preventing heap allocation attack: keep links from a->b and from b->a. Thus making it harder for the attcker to 
actually replace the address space. For control flow we only need to save a->b mapping, but the b->a mapping can be used 
to ensure that this peice of memory is not being maliciously changed by some attacker. 
* __Permission System__ Typically once apps binary has been compiled, its signed by some authority and then before actually 
installing it app will certify this signature. Linux apps are self signed, this way it weakens the security of the app, 
but at least makes someone responsible. But this means security related design is much more imp for Linux.. 

__Malware__ App if it does something you didnt intend for it to do. Violates Confidentiality, integrity or availability 
expectations as a user.  

__Confidentiality__ violation can be determined by static or dynamic analysis on the bytecode. They do data flow analysis
to determine how app behaves. By doing this they are trying to determine source to sink flows of data within the App. 
 

__Web driven mobile Apps__ For example yelp has all the code about to to show the content on the phone in the app, 
but the data on the app is web driven. So what kind of security issues can be present there. 
Benefits: Android and Ios apps will be very similar in this case, because then you made the app platform independent. 

__JavaScript bridge__ lets web content use Java objects exported by app. WebView does not isolate bridge access by 
frame or origin: the bridge used to actually link the web with an app, calls can be made by any component of the javascript 
frame to the web server. => _no Isolation_ . Thus server doesnt get the guarantee that the calls coming to it are from 
valid app or from random objects preset on the App’s web view. 
Thus app doesn't have any guarantee that the calls coming to it are from the valid browser. 
* App environment may leak sensitive web information in URLs
* * Loading untrusted web content: Advice to mobile developers: Whitelist the webpages your app will be talking to, 
otherwise your app can be manipulated to load a url not to the intent of your application. 
* * Avoid Leaking URLs to foreign apps
* * Avoid exposing state changing navigation to foreign apps
When a WebView loads a page, an intent is sent to the app(app will be listening for it) Another app can register a 
filter that might match this intent : Example Google 
* If the URL contains sensitive information, this information can be stolen -> by hacker because the malicious app also 
sets same intent filter as needed to be able to get handle of the auth token/session token. 
* WebView does not provide security indicator
* SSL error : large fraction of mobile apps do not handle SSL errors. And mostly these errors are leaking from commonly 
used libraries. 
* Theft: Locate phone if its lost, locate phone remotely and wipe data in case its stolen, data backup on the cloud.  
 

# iOS Security architecture:
### __Boot process__
How to ensure correct Software starts the phone? 
Every layer checks that the next process to be started is a certified process. (Bootchain)
__Root of Trust__ : boot rom installed during fabrication. Its read only memory of phone and can never be changed. It has apple root public key built into it, and that’s used to verify every next process. 
Boot Rom -> Low level boot loader -> IBoot-> iOS kernel. 
For every process compute hash of the code, verify the hash has been properly signed by apple. 
Jailbreaking : interrupt the root of trust, by finding some gap in here.. 
### Software update: updates are signed by Apple, and before updating, the update is verified on the device. 
The signature contains : Hash of the update code, device Unique id, and nounce from the device. => device id is used so 
that apple can keep apple keeps tracks of which device has which update. => Nonce : ensures that device is always getting 
latest software update. 

### Hardware security features __Secure Enclave__ 
Built inside the processor. using some features of the ARM chip. Chip has : Application processor and one Secure Enclave. 
Enclave has a random number generator to get good entropy needed for security to ensure outside world cannot predict keys. 
It stores keys, UID : unique number not known to outside world. 
Many things which are shared by the enclave, application processes don’t have keys to decrypt it. 
Memory management unit encrypts everything that’s written to flash. The key used to encrypt is not known to the Application 
processor. Secure Enclave can access the memory management unit, thus able to hide info from application process.

### iOS cryptography: Key management and the iOS crypto framework
__Data protection API__ key hierarchy on how data gets encrypted. 
Per file encryption key : AES-XTS
File metadata will contain the key. Part of the directory where the file is present. 
_Class key_ : encrypt per file key,
File meta-data contains the ciphertext is stored in meta data file.  
File-system key : encrypts file meta-data. This key is encrypted by a key generated from Secure Enclave. 
And is a only accessible to Secure Enclave. 
Class keys are protected from passcode keys and from hardware-keys. 

File encryption is done by the application process, that’s fine because application anyways had access to the file. 
However everything else is done by Secure Enclave.. => make everything secure without much overhead. 

Wipe out doesn’t actually delete data from flash, it just deletes the file system key. Thus now you won’t be able to read 
the data.  

Keys hierarchy: 
Class keys: Different files belong to different types of protection 
* __Complete protection__: the key is available when deceive is unlocked. It’s derived from the passcode and device uid. Thus only device will be able to access this one file. Data is locked to the device and to the user. So the device forgets about the class key moment the device is locked. 
Class keys are gone when rebooted (even with touchid user needs to provide the passcode).
* __Protected Until first User authentication__ : (default) class key is not deleted when device is locked. So on reboot the class key is deleted. 
* __No protection__ : class key is never erased, until a device is wiped. Thus files are always available for file read and write. Thus file is encrypted, however the key is always available. 
* __Protected Unless Open__ : even when device is makes use of public key crypto..  

Based on use case application will determine how to set the file protection key for the files. 

Xcode allows you to set the setting for your whole app as one type of class key, thus ensuring that all data from the app to the protection type you want. 

__Keychain__: Apple Database to store sensitive data on the device. Key value pair. 
Different apps would need access to this database, but you want to make sure different apps written by me are available for access for me, however no one else can see them. 
IOS knows apps are from same developer because they are signed by the developer. 

__Access class__ : 
Complete, UntilFirstUserAuthentication, NoProtection
* Thisdeviceonly -> key will be derived from the device ID, thus only the device will be able to decrypt the data. 
Conditions can be set on when the keychain item can read on the items can be performed: eg: only when passcode is entered, or only when user is in certain location.  

Backup : data is backed up in encrypted form. Except the data in NoProtection form. 
Classkeys: backed up protection by iCloud Keys 

### Unlock process 
__Passcode key__ : derived by Hashing  passcode and device ID. This verification is always done in the secure enclave. It’s always involved in securing the passcode. 
Enclave delays every passcode verification by 80 ms. but it can be programmed to delay more, and even force it to erase phone after some tries. (counter is in secure enclave)

__Touchid__ : upon locking the phone, class keys are not deleted, stored in secure enclave encrypted via finger key. => touch id is less secure than passcode.. 
* Sends fingerprint image to secure enclave (encrypted)
* Enclave stores skeleton encrypted with secure enclave key
* Class‐key is stored encrypted by secure enclave
* Decrypted when authorized fingerprint is recognized
* Deleted upon reboot, 48 hours of inactivity, or five failed attempts

### Application level security 
all applications run under a sandbox. run as non privileged user. Access to file system is restricted. Each app is 
randomly provided an area where it can read and write from. Everything else is controlled by iOS. Applications are also 
protected from exploits. 
* NX bit : execute never bit. Mark stack and heap memory pages as non‐execute, enforced by CPU => prevents buffer overflow attacks and memory corruption attacks. 
* ASLR (address space layout randomization)
At app startup: randomize location of executable, heap, stack 
At boot time: randomize location of shared libraries
* Jailbreaking detection : Not very viable, its a best effort only. 
* Application Code signing : All executable code has to be signed by Apple certificate. Though the review process can have 
leaks. 

One hack App : downloaded some data from remote server and saved it in some part of the phone memory which was not protected by the XN bit. Thus he was able to run malicious piece of code. 
Why is there a non‐XN region? Needed for Safari JIT

* Apple store : apps are tried to make secure, because the apps go through a lot of review process done by apple. 
 
* __Application Layer Crypto__ : CCCrypt framework within Ios libraries. Not all of them are great. 

__iMessages security__ 
Support end to end encrypted system. Phone generates 2 private-public key pairs. 
* Encryption keys 
* Signing keys 
Public keys are sent to Apple and stored in Apples iCloud directory service(IDS). Each such device generates this key and then APN server keeps pushing data to these devices using these keys. 
__APN : Apple Push notification service__ 
So to send a msg, Sender first gets the public keys of all devices for the recipient. Then the msg would get encrypted with the devices keys for recipient and signs with sender’s private key. 
The data is encrypted, but metadata is not encrypted. That’s what’s used by companies.. 


* TLS/SSL
Server needs to serve https TLS 1.2 valid certs. iOS Verifies that certs are valid. Invalid certs are not honored. 
App developers can override the transport layer security. In the apps info file app developer can specify a list of URLs 
to access over HTTP. However developer can also deliberately disable the SSL verification. 

__Certificate pinning__  you want to make sure that application can only talk with the cloud service you setup. Embed the certificate in the apps so that in case CA by mistake issues certificate to incorrect site.. test with expired certificate, certificate which has incorrect CA, etc.. If incorrectly done very easily exposes the phone for man in the middle attack. 

* __Delete private State__ : when using NSURLsession . The data gets cached locally, cookies can get cached, on the 
client side. All these things are stored on disk which can become accessible for someone who steals it. 
NSURLsession can be configured to be a ephemeral session => do not write the data to disk and only keep the data in RAM. 

* __WebView__ : initialize a webView object, that area will then used to present HTML data from online server. 
* *  can set JavaScript to be disabled => thus remote server cannot mess up the client. 
* *  hasOnlySercureContent flag : to ensure all the data coming to webview is from SSL. 

* __Data leaks__
* * pasteboard : data is available for every app on the device. Prevent sensitive data from being copied. 
* * IOS snapshots : application is sent to background in case phone comes.. so iOS takes snapshot of the screen and keeps in background to ensure the next time the apps is selected to be in background that snapshot is loaded.. this snapshot is written to disk, then this data is exposed outside of the application by iOS itself. Prevention : up to developer...save the splash image which is a just logo of the app..etc..

### __Geolocation__ : 
* Can be determined using WiFi SSids. Quite fast, low power mode, location is not very accurate. 
* GPS system: using whole bunch of satellites but is accurate to few meters. Though signal might not be able to make it thorough in many cases, it can be spoofed, and takes quite power to determine the GPS signal. 
While coding, developer can note the level of accuracy they want.. 
delete the data from the service as soon as it’s done. Don’t store exact location,  
 
__Location services with tracking__ : need to determine hot-spots, or location based advertising, or location based search, etc. 

Abusing mobile sensors : 
Identity for vendor : to keep track of which device and which user’s account the app is installed. 
Accelerometer: their errors different per device. And apps can use that to determine/pinpoints particular device. 
Power Usage sensor : applications can monitor power usage, but it can be abused to determine location of the device. => by determining how much power is being used by device to connect to cellular tower. It’s a stable fingerprint for every geolocation. Only works when user is moving.. 

## __Mobile Device management__
* HYOD Heres your own device : 
* Choose your own device
* Bring your own device 

__Device management__
Configuration management or hardware mgmt is needed.
What are the different levels of encryption needed on the device to ensure that data is not lost.. 
if device gets lost, company needs to be able to wipe the phone to ensure data is not breached,, 

__Mobile management platform__
Company wants to be able to control some parts of the mobile. 
* Enrollment process: MDM enterprise server registers the device with the users ldap, saves details. Regularly the device gets connected with the MDM server to check things.. 
User needs to confirm he is fine with some changes being made to the device. 
Phone gets lost, and employee reported it the MDM server will wipe out the data from the phone. 

* MDM trade offs : personal data, and personal apps, are part of different area and corp app and corp data are part of different area on the phone.. 

* Virtual machine based approach : 
Have a virtual machine for corporate apps, and corporate VM’s etc. 
Problem : multiple Os on mobile can cause performance impact. Multiple layers will have performance issues : 1. Cache 2. Stacks 
UI issues, technical issues need to be worked out.  
* Virtual Mobile device :
The phone is just the UI/display for things, and everything is done at the server on the machine. => no security issue at all because the client is a dumb client. 
Issue : slow connection with the server means slow performance. 
