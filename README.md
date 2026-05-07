# Mobile Forensics — Android & iOS Device Analysis

This project covers two separate forensic investigations I conducted on mobile device images as part of my coursework at the University of Maryland's Advanced Cybersecurity Experience for Students (ACES) program. Each investigation involved a different mobile platform Android and iOS and a different set of tools purpose-built for each.

The overarching scenario across both labs involved a cyberattack on a student financial loan institution. The suspects Chester and Alan were apprehended, and their devices were seized for analysis. My job was to go through those devices, piece together what happened, and document everything with supporting evidence.

---

## Android Investigation — ALEAPP

For the Android side, I used a tool called ALEAPP (Android Logs Events And Protobuf Parser). It takes an Android device image and parses it into a clean, browsable HTML report, which makes it a lot easier to dig through the hundreds of artifact types an Android device carries.

What I found painted a pretty clear picture. Starting from the factory reset record, I could tell that Chester had wiped the device at some point which is a red flag in any investigation since it means earlier data is gone. From there, I was able to pull his phone number and carrier from the SIM database, confirm his app usage patterns, and identify that Kali Linux was installed on the device through a Snapchat chat exchange with Alan. The bash history on the device backed that up. Nmap was being actively run, which is a port scanning tool you'd use when scoping out a target network.

One of the more interesting finds was in the Gmail database. There was a suspicious file `Chestnut_CV.exe` that Alan sent to Chester, and we were able to confirm it also existed in his Google Drive. Between the Zynga Chess chat logs (yes, attackers sometimes use random apps to communicate) where Alan explicitly named Mallie Sae as their target, and the Chrome history showing Chester searching for Magisk Manager (a rooting tool), we had a pretty well-rounded case.

[Full Android writeup →](Android(ALEAPP)/AndroidAnalysis.md)

---

## iOS Investigation — iLEAPP

The iOS investigation used iLEAPP, the iOS counterpart to ALEAPP. Apple doesn't make it easy for investigators they're famously resistant to cooperating with law enforcement so having a tool that can parse iOS-specific formats like plists, archive files, and Wallet passes is essential.

This device belonged to Alan. Right away the iCloud sync database told us the device name: "Alan's Fantastical iPhone," which helped confirm whose phone we were dealing with. From the SIM and device identifiers, we were able to build out a unique profile for the device. The iTunes backup plist gave us a sense of what data might be missing based on when the last backup was taken.

Things got more interesting from there. The Medical ID artifact handed us Alan's date of birth January 20, 2000 which is the kind of personal detail that can unlock accounts or answer security questions. His flight pass data showed a travel route from Florida to New York to Vermont, giving us a physical timeline we could work with. The iTunesPrefs file told us he had connected and authorized a desktop computer `jfarl - DESKTOP-A108NFK` which could be another source of evidence worth pursuing.

The SMS database is where the most direct evidence was. The conversations covered hacking, jailbreaking the phone using a tool called Substrate, and picking out a target. And the image cache database was perhaps the most damning it held images containing private personal information on multiple individuals, including student loan records and names. That's not something you'd have unless you took it.

[Full iOS writeup →](iOS(iLEAPP)/iOSAnalysis.md)

---

## What I Took Away From This

Going through two different platforms back-to-back really highlighted how differently Android and iOS store and structure their data. Android is a lot more open there's a wider variety of artifacts, more file types, and more paths an analyst can take. iOS is more locked down by design, but that actually makes the artifacts you do find feel more deliberate and significant.

Both ALEAPP and iLEAPP were genuinely impressive in how much they surface from a raw image file. A lot of forensic work can be tedious, but having a tool that organizes everything into a navigable report makes the analytical part figuring out what the data means and how it connects a lot more manageable.

---

## Tools Used
- [ALEAPP](https://github.com/abrignoni/ALEAPP) — Android forensic artifact parser
- [iLEAPP](https://github.com/abrignoni/iLEAPP) — iOS forensic artifact parser

## Structure
```
├── Android(ALEAPP)/
│   └── AndroidAnalysis.md
├── iOS(iLEAPP)/
│   └── iOSAnalysis.md
└── images/
    └── (all supporting screenshots referenced in both writeups)
```
