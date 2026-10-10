# મારું ઘર (MARU GHAR) — Website + Android APK source

આ સોર્સ ગાંધીનગર, ગુજરાત માટે છે. આ **પ્રોજેક્ટ સોર્સ** છે; Firebase જોડ્યા, rules deploy કર્યા, hosting publish કર્યું અને Android build ટેસ્ટ કર્યું હોય એવું માનવું નહીં.

## હાલના સોર્સમાં
- ગુજરાતી, મોબાઇલ-ફ્રેન્ડલી જાહેર વેબસાઇટ.
- ઘર, ફ્લેટ, દુકાન, રહેણાંક/કોમર્શિયલ પ્લોટ, ઓફિસ અને વેરહાઉસ કેટેગરી.
- મંજૂર થયેલી લિસ્ટિંગ માટે real-time Firestore listener.
- માલિકનું listing form; નવી લિસ્ટિંગ `pending` રહે છે.
- 4 સુધી ફોટા અપલોડ કરવાનો પ્રારંભિક flow (દરેક 5 MBથી ઓછો; JPG/PNG/WEBP/GIF).
- ફોટા અને સંપર્ક નંબર મંજૂરી પછી જાહેર થશે તે માટે માલિકની ફરજિયાત સંમતિ.
- પ્રારંભિક ગોપનીયતા નીતિનો ડ્રાફ્ટ `public/privacy.html`માં.
- Admin email/password login અને UID allow-list ચેક; pending listing approve/reject.
- Firestore અને Storage rulesનો પ્રારંભિક કડક સેટ.
- Capacitor Android wrapper અને GitHub Actions debug APK workflow.
- સંપર્ક નંબર: 6357030198, 9737296498, 7016873003.

## 1. નવો Firebase પ્રોજેક્ટ બનાવો
1. Firebase Consoleમાં નવો project બનાવો — ઉદાહરણ તરીકે `maru-ghar-gandhinagar` (નામ ઉપલબ્ધ હોવું જરૂરી).
2. Project settings > Your apps > Web appમાં app ઉમેરો.
3. Firebase આપેલી config values `public/firebase-config.js` માં મૂકો. `storageBucket` સહિત દરેક value Consoleમાંથી જ copy કરો; bucket નામ અંદાજથી ન લખવું.
4. Authentication > Sign-in methodમાં **Email/Password** ચાલુ કરો અને એક admin user બનાવો.
5. Authentication > Usersમાંથી admin userનો UID copy કરો.
6. એ જ UID `public/firebase-config.js`ના `adminUid`, `firestore/firestore.rules` અને `storage/storage.rules`માં `REPLACE_WITH_ADMIN_UID`ની જગ્યાએ મૂકો.
7. Firestore Database બનાવો અને Storage ચાલુ કરો.
8. Firebase Consoleમાં App Check સેટ કરીને web app માટે યોગ્ય provider જોડો અને પરીક્ષણ કર્યા પછી enforcement ચાલુ કરો. Public listing form હોવાથી abuse protection જરૂરી છે.

## 2. Firebase project ID સેટ કરો
`.firebaserc`માં `REPLACE_WITH_YOUR_FIREBASE_PROJECT_ID`ને સાચા Project IDથી બદલો. Project ID, display nameથી અલગ હોઈ શકે છે; Consoleમાં દેખાતો Project ID જ વાપરો.

## 3. સ્થાનિક પરીક્ષણ / Hosting deploy
Node.js 20+ અને Firebase CLI જરૂરી છે.

```bash
npm install
npx cap --version
npm install -g firebase-tools
firebase login
firebase use --add
firebase deploy --only hosting,firestore:rules,storage
```

`firebase use --add` વખતે આ જ નવા projectને પસંદ કરો. Deploy કરતાં પહેલાં rules અને UID ફરી તપાસો. Firebase Hostingમાં `public/` directory deploy થશે; admin page `/admin.html` પર રહેશે.

## 4. GitHub પરથી Android debug APK
1. ZIP extract કરો અને project files નવી GitHub repositoryના rootમાં upload કરો.
2. ખાતરી કરો કે `public/firebase-config.js`, `firestore/firestore.rules`, `storage/storage.rules` અને `.firebaserc`માં project values યોગ્ય છે. **Firebase service-account key, password અથવા signing key repositoryમાં ન મૂકો.**
3. GitHub repository > Actions ખોલો.
4. **Build Maru Ghar Android APK** workflow પસંદ કરો અને **Run workflow** દબાવો.
5. Workflow સફળ થયા પછી runની અંદર `maru-ghar-debug-apk` artifact download કરો.

આ debug APK છે; Play Store માટે signed release, branded app icon, privacy policy, device testing અને release security review અલગથી કરવાના છે.

## જરૂરી સાવચેતી
- `public/firebase-config.js`માં placeholder values હોય ત્યાં સુધી Firebase features કામ નહીં કરે.
- Firestore/Storage rulesમાં UID placeholder રહી જાય તો admin કાર્યો નિષ્ફળ જશે; તેને જાહેર ન કરો.
- Firebase Web API key સામાન્ય રીતે client configનો ભાગ છે; વાસ્તવિક સુરક્ષા Authentication, Security Rules અને App Check પર આધારિત છે.
- Public listing submission abuse થવાની શક્યતા છે; App Check અને production monitoring અનિવાર્ય રીતે configure/test કરો.
- Owner listing અને photos pending રહેશે. Admin મંજૂર કરે ત્યાર પછી જ public queryમાં listing આવશે.
- APK online Firebase services પર આધારિત છે; આ offline-first app નથી.
- આ sourceને અહીંથી Firebase project અથવા GitHub repositoryમાં deploy કરવામાં આવ્યો નથી. Build અને live testing સફળ થયા પછી જ deploymentને complete ગણો.


## Cloudinary image uploads (configured for this ZIP)
- Cloud name: `t5gd6nts`
- Unsigned upload preset: `marughar_images`
- The public site and Android wrapper upload images to Cloudinary and store each returned HTTPS image URL in Firestore `listings.photoPaths`.
- The Cloudinary API Secret must never be placed in HTML, JavaScript, Android assets, or a repository.
- Important security note: unsigned upload presets are callable by anyone who discovers the preset. The browser's 4-image / 5 MB checks can be bypassed. Before public production use, configure restrictive upload preset limits/allowed formats in Cloudinary and consider a server-side signed upload endpoint. This ZIP does not implement server-side signing.
- Firebase Hosting, Firestore rules, the live Firebase connection, Cloudinary upload, and Android APK build have not been deployed or live-tested by this package generation step. Test with a small non-sensitive test image before launch.
