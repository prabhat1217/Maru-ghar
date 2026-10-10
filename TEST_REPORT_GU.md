# પ્રારંભિક સોર્સ તપાસ રિપોર્ટ

તારીખ: 10-10-2026

## આ પર્યાવરણમાં તપાસ્યું
- `package.json`, `firebase.json`, `capacitor.config.json`નું JSON parse.
- `public/index.html` અને `public/admin.html`ના JavaScript module syntax માટે Node.js `--check`.
- જરૂરી ફાઇલ/ફીચર હાજર છે તેની static checks.
- ZIPમાં project filesનો સમાવેશ.

## હજી ચકાસી શકાયું નથી
- Firebase project/config ઉપલબ્ધ નથી, તેથી વાસ્તવિક login, Firestore rules deployment, photo upload, approval અને live sync test કરેલા નથી.
- GitHub repository/Actions run ઉપલબ્ધ નથી, તેથી Android APK build આ પર્યાવરણમાં સફળ થયેલો નથી.
- મોબાઇલ/કમ્પ્યુટર પર browser visual testing અને Android device installation testing બાકી છે.
- Firebase rulesનું Firebase Emulator અથવા deployed project પર runtime validation બાકી છે.

## લોન્ચ પહેલાં ફરજિયાત
1. Firebase config અને admin UID placeholders બદલો.
2. Firestore/Storage rules deploy કરીને Emulator/real projectમાં negative/positive access tests કરો.
3. Firebase App Check configure કરો.
4. Admin-only approve/reject, unauthenticated pending submit, approved-only public read અને photo visibility test કરો.
5. GitHub Actions debug APK build ચલાવી APK ઇન્સ્ટોલ કરીને Android device પર તપાસો.
6. Privacy policyમાં વાસ્તવિક ડેટા-હેન્ડલિંગ વિગતો અને support email/website વિગતો ઉમેરો.


Cloudinary integration update: source now posts selected images to the Cloudinary unsigned upload endpoint using cloud name `t5gd6nts` and preset `marughar_images`; the returned HTTPS URLs are stored in Firestore `photoPaths`. Source-level syntax and ZIP checks are required after this update. Live upload and Firebase permissions are not verified. Unsigned-upload abuse risk remains; see README_GU.md.
