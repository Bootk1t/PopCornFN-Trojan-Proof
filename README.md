# PopCornFN Trojan Proof
<div style="font-size: 6px;">
File : PopCornFN.exe  (35 MB, PE32+ x86-64, unsigned)
SHA256 : 8f9dd3fd86fa69a835405678380515058cd33167a91fc726fff54244aa784b92
MD5 : ac3632745fb0af1a9fb8f39d292eafab
Src : fileport.io RAR -> Cracked/PopCornFN.exe


WHAT IT IS
NOT a Fortnite crack. Trojan / infostealer / MITM proxy.
Packed with PyInstaller (strings: MEI, PYINST, PyConfig, python).
Bundled libs: mitmproxy, ldap3, requests, cryptography, trio, zmq.
PDB leak: C:\Users\indom\...\red_hat.pdb -> proj "red hat", dev "indom".


KEY SECTIONS
.text ent 6.46  .rdata ent 6.99  .data ent 7.94  .rsrc ent 7.9972 (33 MB, encrypted blob)
TLS callbacks + IsDebuggerPresent -> anti-dbg. No Rich Header, no sig.


KEY IMPORTS
WS2_32 : sockets (connect, send, recv, ...)
CRYPT32 : certs, schannel, pinned pub key
ADVAPI32 : crypto + RegSetValueExA (reg write)
KERNEL32 : CreateProcess, WinExec, ShellExecute, LoadLibrary
USER32 : clipboard theft (Open/Get/Empty/SetClipboardData)
WININET : proxy hijack
WLDAP32 : LDAP/AD recon (ordinal-hidden)


NETWORK / IOCs
URLs :
  bstlar.com/keys/validate/   <- license check
  bstlar.com/4jl/popdevkey
  discord.gg/z8sZ9k2prv
bstlar.com = Boostellar link locker (ad monetizer for game cracks, reg Oct 2022, Cloudflare, URLert 0/100 high risk).
IPs : only 127.0.0.1 real. Others are cert OIDs.


BEHAVIOR
1. Loader runs, TLS callbacks fire first (anti-dbg).
2. Extracts 2nd stage PE from .rsrc (/EXE/102, 34 MB, MZ, PyInstaller overlay).
3. Runs Python app w/ mitmproxy -> MITM HTTPS, proxy hijack (HKLM Internet Settings).
4. Clipboard theft.
5. Kills FortniteClient, FortniteLauncher, EpicGamesLauncher, ..._EAC_EOS.exe.
6. LDAP/AD recon via ldap3 + WLDAP32.
7. Beacons bstlar.com/keys/validate/.
8. Drops C:\ProgramData\popcorn_mcp_config.json + red_hat.bin.
9. Service: popcorn_svc_*.exe (persistence).


IOC LIST
Hashes : SHA256 8f9dd..., MD5 ac363..., SHA1 2cf5a...
2nd st : 5b55b3482ae1576839a91ffc5bf07911
Domains : bstlar.com, discord.gg/z8sZ9k2prv
Files : C:\ProgramData\popcorn_mcp_config.json, C:\ProgramData\red_hat.bin, popcorn_svc_*.exe
Reg : HKLM\...\CurrentVersion\Internet Settings


VERDICT
Class : Trojan / infostealer / MITM proxy (fake Fortnite crack)
Risk : HIGH
Reasons : unsigned, packed (ent ~8.0), anti-dbg, mitmproxy MITM, clipboard cred theft, LDAP recon, kills EAC/Fortnite, C2 beacon, persistence, spread via warez (0x21cracks.com) + ad link locker (bstlar.com).

