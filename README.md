# VTuber OPSEC Playbook 
*Author: HaittaNeo, 2025*

---

## Opening Remarks  

Hello everyone, 
I’m Neo known across my socials as **HaittaNeo**—and tonight we’re tearing back the blackout curtain on the security posture that keeps my information secure and how you can apply some of these tips to your security posture.   

This presentation is not a list of paranoid life hacks.  
It’s a discipline.  
Think of it as the digital equivalent of stagecraft: every prop, every light cue, every camera angle is intentional.  
Adopt what fits your threat model, discard what doesn’t, and remember that perfection is impossible—but obscurity is scalable.

---

## Section 1 — Why Anonymity Matters  

Visibility fuels growth. Privacy preserves safety.  
As VTubers we profit from parasocial intimacy—the feeling that fans could knock on our bedroom door at any moment and we’d welcome them in. That same illusion is exactly what a doxxer weaponises when they decide the show isn’t enough and they want the actor behind the mask.  

Doxxing is no longer a fringe prank. It’s an entire economy of data‑broker APIs, breach markets, and spite‑driven Discord servers.  
A single résumé PDF from college or a reflected street sign in your posts can be enough for a motivated troll to triangulate your front porch.  

Tonight we answer the question, **“How do I let millions of strangers into my virtual bedroom without inviting even one of them into my real hallway?”**

---

## Section 2 — Our Threat Model (Expanded)

> By classifying attackers and studying their playbooks, we can build defenses that matter in the real world—not in a vacuum.

| # | Adversary | Typical Personas | Hunting Style & Tool-Chain | Concrete Example | Core Goal |
|:-:|-----------|-----------------|---------------------------|------------------|-----------|
| 1 | **Open-Source Investigators**<br>(OSINT hobbyists, gossip subs) | • Drama-sub moderators<br>• Wiki editors<br>• Puzzle-solving fans | 1. **Google dorks** → `filetype:pdf resume "first_last"`<br>2. **Wayback Machine** for deleted Tumblr posts<br>3. **EXIF scrapes** on art Discord to extract GPS<br>4. Cross-link old gamertags with new alias | Hololive fan finds a debut slide’s background JPEG in a 2013 deviantART post listing the artist’s full legal name & hometown. | Public bragging rights / clout on KiwiFarms, Reddit, X |
| 2 | **Opportunistic Criminals** | • Dark-web “combo-list” resellers<br>• Credential-stuffing bot-renters<br>• Low-skill carders | 1. Buy leaked Twitch or Patreon dump<br>2. Feed combos into **OpenBullet** configs<br>3. Check Netflix, PayPal, Coinbase for matches<br>4. Resell or cash-out gift-card balances | After the 2021 Twitch breach, botters reused creator e-mails in Office 365 sprays, hijacking PayPal accounts for gift-card fraud. | Fast monetisation: gift cards, BTC, resale logins |
| 3 | **Social Engineers** | • Fake sponsor agencies<br>• Impersonated VTuber managers<br>• “Tech-support” reps | 1. Spoof e-mail `brand-collab@outlook.com`<br>2. Attach “NDA.pdf” ⇒ hidden credential phish<br>3. DM via Discord, lure to malvertising page<br>4. Steal Google/Discord OAuth; pivot to Twitch | “G-Fuel Manager” sent DocuSign links to 30 VTubers; the OAuth proxy stole cookies, flipped channels to crypto-scam livestreams. | Credential theft → channel takeover → ad-spam / ransom |
| 4 | **Physical Correlators** | • Stalkers, jilted fans<br>• IRL stream-snipers<br>• Swatters | 1. Zoom glasses reflection; match skyline in Google Street View<br>2. **Shazam** a street jingle; map ice-cream routes<br>3. Extract shipping label frame-by-frame<br>4. SDR scan for 5 GHz Wi-Fi BSSID | Fan froze a 4 K unboxing frame, read partial UPS label, narrowed address; then wardrived until router SSID matched. | Acquire GPS location for IRL meetup, swatting, blackmail |

### Key Take-Aways

* **Same data, different motives.** A harmless résumé PDF fuels OSINT clout *and* identity fraud.  
* **Tool overlap.** Social engineers begin with OSINT dorks to craft convincing pretexts.  
* **90 % coverage.** Harden against these four archetypes—public leaks, credential reuse, phishing, physical breadcrumbs—and you defeat most realistic attacks.


---

## Section 3 — The C.L.O.A.K. Framework  

1. **Compartmentalize** Separate devices, accounts, browser profiles.  
2. **Limit Exposure** Share only mission‑critical data—no kids’ names, no hometown.  
3. **Obfuscate** Aliases, P.O. boxes, RTMP relays, Series‑LLCs.  
4. **Audit** Red‑team yourself monthly—breach scans, Google dorks, mod‑log reviews.  
5. **Kill‑Switch** Incident playbook: DMCA templates, local PD “false‑report” flag, lawyer on speed‑dial.

Everything else in this talk hangs from one of those five pillars.

---

## Section 4 — Building a Hardened Digital Identity  

When I debut a persona I create four things:

1. **Root mailbox** on Proton. The root mailbox never appears in public. Every outward‑facing address is a *Proton alias*—You get up to 15 aliases on a paid Proton plan, and each lives or dies with the platform it serves.  
2. **Hardware MFA key** (YubiKey 5C NFC) bound to root and critical services.  
3. **Reserved usernames** locked on every major platform.  
4. **Zero reuse rule**: no old gamer tags, no childhood e‑mails, no recycled passwords. The cleanest identity is grown from sterile soil.

For passwords I rely on **iCloud Keychain**—20‑plus‑character random strings, end‑to‑end encrypted across my Apple gear, exportable for offline backups.

---

## Section 5 — Outsmarting the OSINT Hunters  

### 5.1 Google Dorking — Dox Yourself First  

Google’s advanced search operators (“dorks”) let you slice Google’s index by **domain, URL path, file type, or exact phrase**.  
Because the public index often mirrors forgotten servers and cloud buckets, these operators are gold for doxxers and red-teamers.

Below are the four example dorks and what each part does.

| Dork Query | Operator(s) Highlighted | What It Forces Google to Do | Why an Attacker Cares |
|------------|-------------------------|-----------------------------|-----------------------|
| `"haittaneo"` | `"<exact phrase>"` (quotation marks) | Return **only** pages containing the *literal* string `haittaneo` (case-insensitive). No stemming, no synonyms. | Confirms how many sites mention the alias—great first recon sweep. |
| `site:twitch.tv "haittaneo"` | `site:` + quotes | Limit results to `twitch.tv` **and** require the literal alias somewhere on the page. | Surfaces every Twitch clip, chat replay, or About section that references the user—useful for finding leaked VODs or comments. |
| `filetype:pdf "yourlastname"` | `filetype:` + quotes | Show only **PDFs** that contain `yourlastname`. Google indexes text inside most PDFs. | Old résumés, invoices, or college papers often sit forgotten on public servers; one PDF can reveal legal name, phone, hometown. |
| `inurl:"uploads" "neo_model"` | `inurl:` + quotes (twice) | Return pages whose **URL** contains the word `uploads` *and* whose page *content or URL* contains `neo_model`. | “uploads” is a common path on poorly-secured corporate CMSes and hobby sites. Pairing it with a model or asset name reveals unlisted PNGs, ZIPs, or PSDs that might still embed EXIF GPS or author tags. |

#### Putting It Together

1. **Recon sweep** → `"haittaneo"` (phrase)  
   *Quick count of public mentions; copy interesting domains.*

2. **Platform enumeration** → `site:twitch.tv "haittaneo"`  
   *Find every Twitch artifact—even deleted VODs still cached.*

3. **Legal-name fishing** → `filetype:pdf "YourLastName"`  
   *Search college portals, resume sub-sites, HOA minutes.*

4. **Asset hunt** → `inurl:"uploads" "neo_model"`  
   *Locate raw art files or rigging PSDs that might store author metadata.*

> **Defensive takeaway:** run these exact dorks against yourself. Anything surprising that turns up should be deleted, 410’ed, or overwritten with non-identifying “white-noise” files, because attackers use the same playbook—just less politely. The point here is trying to correlate your vtuber persona to your real identity 


### 5.2 YouTube Business‑Email Leak  

Open **About → For business enquiries → View e‑mail address** on a channel and you’ll see the creator’s inbox. If that’s an old Gmail—or worse, one tied to real‑life log‑ins—a single click links your persona to your legal identity. Publish **only** a Proton alias dedicated to VTubing and rotate it yearly. The reason we want this email to be private will be discussed in the databrokers down below. 

### 5.3 Leak‑Check via Have I Been Pwned  

Search every alias at <https://haveibeenpwned.com>.  
A hit means that database—password hashes, IP logs, sometimes physical addresses—is loose on dark‑web spreadsheets as well as service web services that do not hide the full details like haveibeenpwned does. Companies *should* salt + bcrypt; many still ship SHA‑1 or plain text. Treat any breach as radioactive:

* Rotate passwords immediately.  
* Retire the alias.  
* Assume attackers can pivot from that data to new accounts.

By using tiny details from your name, username, email, IP address, VIN, and more, I can easily access leaked databases and view all of your information. I can also curate a large list of passwords you might use, based on the old passwords I already have.
For a previous project, I created a tool that ingested those old passwords, then asked for additional data—birthdays, pet names, family names, etc.—and generated an extensive list of likely passwords you could be using.

### 5.4 Data‑Broker Scrub — Nuke Your IRL Footprint  

> **Data brokers**—Spokeo, Whitepages, Intelius, BeenVerified, YellowPages, MyLife, etc.—package and sell your **full name, address history, relatives, DOB, salary band** for pennies. Stalkers love them.

#### 5.4.1 Manual Opt‑Out Walk‑Through  

| Broker | Removal Path | Confirmation |
|--------|--------------|--------------|
| Spokeo | <https://www.spokeo.com/opt_out_form/new> | E‑mail link |
| Whitepages | Profile bottom → **Remove My Info** | SMS code |
| Intelius / BeenVerified | Each site’s opt‑out form | E‑mail link |
| YellowPages | E-mail `removeme@yellowpages.com` | CSR reply |

Track each URL + date removed; calendar re‑check every 90 days.

#### 5.4.2 Automated Service — DeleteMe  

| Feature | Benefit |
|---------|---------|
| 750 + brokers covered | Hits sites you’ll never find manually |
| Quarterly privacy report | Shows which broker re‑listed you |
| Google cache removal | Scrubs stale search snippets |

Cost ≈ \$129/yr single. Time > money? Subscribe—still spot‑check twice a year.


---

## Section 6: Fortify Your Device & Accounts  

### 6.0 Two User Profiles, One Wall  

| Account | Use It For | What *Never* Happens |
|---------|------------|----------------------|
| **IRL Profile** | Banking, taxes, personal socials | OBS, Twitch, VTuber Discord |
| **VTuber Profile** | OBS, gaming, sponsor e‑mail, VTuber Discord | Family Facebook, personal Amazon |

*Windows:* Settings ▸ Accounts ▸ Family & other users ➜ **Add user without Microsoft account** → name `VTuber_name`.  
The main term for what were doing here is segmentation. The purpose is that if one user account gets breached or gets a virus from an attacker it cant see the other side. So if your on your vtuber user checking your discord and make a mistake it wont reveal any of the information stored in your IRL profile. 


### 6.1 Baseline Defences  

* **Defender** real‑time + **Malwarebytes** weekly Threat Scan.  
* **Windows Update**: “Receive other MS products” ON; auto‑reboot.  
* **NordVPN** in VTuber profile; Auto‑connect + Kill‑switch.  
* Download files into `C:\VT_Dropbox`, scan with Defender, then VirusTotal.

### 6.2 Attack Families & Counter-Play — Deep Dive

| # | Class | Typical Delivery & Red Flags | How the Payload Works | Step-by-Step Counter-Play |
|---|-------|-----------------------------|-----------------------|---------------------------|
| **1** | **Phishing / Pretext E-mails** | *Red flags:* <br>• “Urgent invoice due TODAY” subject line <br>• Sender domain looks off (`paypal-support.com` vs. `paypal.com`) <br>• Links masked behind **bit.ly** or **drive.google.com/uc?export=download** | HTML attachment loads fake login page → steals credentials; or link redirects through three domains to obscure reputation. | 1. **Preview headers** (⇧⌘H in Gmail; “File ▸ Properties ▸ Internet headers” in Outlook). SPF **and** DKIM must be *PASS*. <br>2. Hover link → copy-link-address → paste in <https://www.virustotal.com/gui/url>. <br>3. If unsure, open the URL in **Browserling** or a disposable VM—never your daily browser. |
| **2** | **Credential / Info-Stealer Malware** *(RedLine, Vidar, Lumma)* | *Red flags:* <br>• You’re suddenly logged out of Discord/Twitch. <br>• Friends message: “Did you just send me a giveaway link?” <br>• New login location in Google “Your devices” security panel. | EXE or DLL injects into `explorer.exe` → grabs browser cookies, passwords, crypto wallets → exfiltrates to Telegram bot or C2 panel. | 1. **Isolate the host** (pull Ethernet, disable Wi-Fi). <br>2. From **another clean device**, change passwords + revoke sessions. <br>3. Boot infected PC → run **Malwarebytes Threat Scan** + **Microsoft Safety Scanner**. <br>4. Check `%APPDATA%`, `%TEMP%`, `C:\ProgramData` for timestamped folders; delete + rescan. |
| **3** | **Discord “Image” Malware / Token Grabbers** | *Red flags:* <br>• File named `pic.png` downloads as `pic.png.exe` (double-extension). <br>• CDN link size > 1 MB for a “PNG”. <br>• Windows shows default EXE icon if extensions are visible. | On execution: steals Discord `%APPDATA%\discord\Local Storage\leveldb` > uploads token > turns your account into spam bot or nukes servers. | 1. Discord ▸ Settings ▸ Advanced ▸ **Disable “Auto-open files after download.”** <br>2. Windows Explorer ▸ View ▸ **File name extensions ✓** (catch double-ext). <br>3. For suspicious attachments, upload to **Hybrid-Analysis** or **Any.Run** sandbox. |
| **4** | **Browser Extension Hijacks** | *Red flags:* <br>• “This extension now requires permission to read all data on every site.” <br>• Extension suddenly vanishes from Chrome Web Store. | Malicious update injects JS on login pages → keylogs + session cookie theft. | 1. Chrome ▸ Extensions ▸ Details ▸ **View in Web Store**. If page 404s, uninstall immediately. <br>2. Keep a monthly screenshot of installed extensions; diff for newcomers. <br>3. Use **Extension Source Viewer** to diff source code between versions. |
| **5** | **Malvertising / Drive-By Downloads** | *Red flags:* <br>• Typo-squatted domain (`obs-project[.]site`). <br>• Browser flashes a download without user click. | Exploit kit fingerprints browser → if unpatched, drops DLL or MSI via PowerShell `Invoke-WebRequest`. | 1. Use **uBlock Origin** with *Malvertising Filter* enabled. <br>2. Keep all browsers + JavaScript engines patched (enable auto-update). <br>3. Verify hashes of installers (OBS publishes SHA-256 on GitHub Releases page). |
| **6** | **Man-in-the-Middle Wi-Fi Traps** *(Conventions, cafés)* | *Red flags:* <br>• Captive portal asks for Google login credentials. <br>• Browser security panel shows “Certificate not valid.” | Rogue AP clones SSID; TLS downgrades or captive-portal phishing harvests cookies. | 1. Use **NordVPN auto-connect** on boot; killswitch blocks traffic if VPN fails. <br>2. Disable auto-connect to open Wi-Fi on both user profiles. <br>3. Carry a GL-iNet travel router; tunnel all devices through its WireGuard. |

---

#### Quick-Reference Flowchart — What to Do When Something Feels Off

```text
Suspicious E-mail / File / Link?
          |
          v
1. VirusTotal URL + hash
          |
          v
>= 1 Red Engine? ------ No --> Proceed in sandbox VM
          |
         Yes
          v
Delete file · Block sender · Report spam
```


---

## Section 7 — Social Media Governance  

### 7.1 No Crossover  

Real‑life contacts **never** engage with VTuber posts.

### 7.2 Audit Cadence  

* **Weekly** — Discord Audit Log.  
* **Monthly** — Export Twitch Shield‑Mode logs; diff against shared ban list.  
* **Quarterly** — Pay \$25 bounty to social‑engineer a mod.

### 7.4 Metadata Reality Check  

Twitter/TikTok scrub EXIF; **Discord, Telegram, Google Drive do not**.

```bash
exiftool -all= promo.png
```

Upload the scrubbed file.

---

## Section 9 — Financial & Legal Veils (Receiving *and* Paying Without Doxxing Yourself)

> Revenue is the lifeblood of a creator business—and, paradoxically, a data-leak super-highway.  
> The goal: let money flow while your real name and home address stay offline.

---

### 9.1 Incoming Money — How to Get Paid Privately

| Revenue Stream | Private Setup | Why It Hides You |
|----------------|--------------|------------------|
| **Donations / Tips** | **PayPal Business** account registered to your LLC’s name → “Customer statement name” set to studio brand (e.g., `NEO-OPS MEDIA`). | Donor’s receipt shows *brand* + LLC EIN; never shows your legal name or residential address. |
| **Patreon / Ko-fi** | Create a **Creator entity** in Patreon settings → enter LLC EIN and virtual mailbox address. | Your W-9 on file uses LLC’s info; patrons see only public display name. |
| **Ad revenue & sponsorship wires** | Use **Stripe Treasury** or **Payoneer** virtual bank account in the LLC’s name; provide EIN on W-8/W-9. | Brand-facing invoice carries routing & account numbers but zero PII. |

> **Don’t:** accept PayPal “Friends & Family,” Cash App, or Venmo. Those surface your personal profile by default.

---

### 9.2 Outgoing Money — Paying Vendors Without Traceback

| Need | Private Method | Extra Benefit |
|------|---------------|---------------|
| Software subscriptions | **Privacy.com** virtual VISA cards created under LLC debit funding. Use a fresh card for each vendor. | Spend limit + auto-lock stops stealth billing. |
| Hardware / merch prototypes | Use a **virtual mailbox** (e.g., Traveling Mailbox) that offers “scan, forward, discard” services. | Your house never appears on a shipping label. |

---

### 9.3 LLC Privacy—It’s Not Magic

| Formation Route | What’s Public | How to De-Identify |
|-----------------|--------------|--------------------|
| **Wyoming / New Mexico / Delaware** LLC with registered agent | State site lists **LLC name, filing date, registered agent address**. *No* manager/member names required. | File through an agent (Northwest, Harbor) and use their address. |
| **Series LLC (WY)** | Public record lists **parent LLC only**. Individual “Series A, B…” are *internal*. | Each revenue arm (Patreon, merch, Twitch) can use its own series EIN; invoices show Series label + parent RA address. |
| **Florida / California** LLC | State requires **manager/member names** in public SunBiz/SOS search. | Instead, create a *holding LLC* in WY → that WY LLC becomes the CA manager. Public site shows *only* the WY entity. |
| **Nominee Manager** | Some states let you appoint a **nominee** (law firm) as the public manager. | Adds cost ($100–300/yr) but hides your legal name even in disclosure-heavy states. |

> **Gotcha:** If you register your LLC in a state that requires you to identify all I have to do is google that states LLC and search for your LLC which is free to do and see all your info. Even if they don't know your state they have 50 tries and I assure you they have the free time to find it. 

---

## Closing Thoughts  

Perfect anonymity is a mirage, but you can raise discovery costs until attackers move on. Live inside **C.L.O.A.K.**—Compartmentalize, Limit, Obfuscate, Audit, Kill‑Switch—and your castle still throws one hell of a party every stream night.

Stay safe, stay anonymous, and keep creating. 
