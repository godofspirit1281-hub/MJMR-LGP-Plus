MJMR LGP+ — Local Guardian Protocol (prototype)
This is a real Android source project, not a compiled APK. It is a first-stage DNS-only VPN ad/tracker filter. It requires user-approved Android VPN access. It routes only the virtual DNS server (10.77.0.1/32) through the VPN; ordinary network traffic is not tunneled.
Build
Open in Android Studio (JDK 17; Android SDK 35; Android Gradle Plugin 8.7.3) and run Build > Build APK(s), or install Gradle 8.9 and run gradle assembleDebug. Output: app/build/outputs/apk/debug/app-debug.apk. No APK has been compiled or tested in this environment.
Building in Termux on ARM64 requires an Android-compatible AAPT2 and SDK configuration. The standard Google Linux SDK command-line tools may not run on Android ARM64. Do not trust screenshots claiming a successful build.
Actual implemented behavior
Native Android app screen: enable/disable protection, status, editable domain blocklist, blocked/forwarded counters.
Android VpnService permission and foreground service notification.
IPv4 UDP DNS requests to the VPN-provided DNS server are blocked (NXDOMAIN) if matching a local domain/suffix rule, otherwise forwarded to Cloudflare DNS at 1.1.1.1 over ordinary UDP/53.
Counters reflect processed requests; they are not estimates of ads removed or bytes saved.
No HTTPS endpoint, remote telemetry or backend is required. No account or personal data collection is implemented.
Limitations / security
Prototype only. Not independently tested. Do not rely on it for security or privacy guarantees.
DNS-only: no filtering of encrypted DNS (DoH/DoT), DNS-over-TCP, IPv6, browser-rendered pop-ups or ads served from first-party domains.
Upstream DNS is unencrypted UDP. Do not label it secure/encrypted DNS. Future version should add verified DoH support.
May fail with certain network configurations, IPv6-only networks, Android Private DNS, other VPN apps or Android version-specific behavior.
Local rules are simple domain suffixes; no auto-updating trusted lists, malware classifier or per-app controls yet.
Android background lifecycle and notification behavior require device testing. Connection status is process-local, not a durable VPN health check.
No Play Store listing, QR installation link or signed release package exists.
Build an APK without Termux Gradle (GitHub Actions)
The included .github/workflows/build-apk.yml uses an Ubuntu runner, JDK 17, Android SDK 35, and Gradle 8.9 to compile the app. This bypasses the reported ProtoLogNative crash on Termux ARM64.
Create a private GitHub repository and upload the contents of this project's root folder (including the hidden .github directory).
Open Actions > Build MJMR LGP+ Android APK > Run workflow.
If the workflow completes successfully, open the run's Artifacts and download MJMR-LGP-Plus-debug-APK.
Extract the ZIP and install app-debug.apk on your Android device after reviewing the source and Android VPN consent prompt. Android may ask you to allow installation from the chosen file manager.
Test DNS resolution, blocklist behaviour, and connection stability. If the workflow fails, use the job log to diagnose; an APK is not guaranteed.
The debug APK is signed with an automatically generated debug key and is suitable for local testing, not production distribution. Never assume an uploaded artifact exists until GitHub reports a successful build. The app is a DNS-only prototype, not a general-purpose HTTPS proxy or full VPN.
