# Post-Quantum digital signature app
This is an Android app, created as a proof of concept for digital signing with post quantum algorithms selected by [NIST](https://csrc.nist.gov/projects/post-quantum-cryptography/round-3-submissions).

It is part of the paper I wrote about the implementing of Post-Quantum-Cryptography Algorithms. 
It got published in the Conference for Omni-Layer Intelligent Systems 2023 and you can read it [here](https://doi.org/10.1109/COINS57856.2023.10189254).

## Description
PQSigningApp is created as an unofficial test app for the [open-quantum-safe/liboqs](https://github.com/open-quantum-safe/liboqs) open source C library for quantum-safe cryptographic algorithms created by [oOpen Quantum Safe project](https://openquantumsafe.org). It is implemented through the Java wrapper [open-quantum-safe/liboqs-java](https://github.com/open-quantum-safe/liboqs-java).

The project is split into two parts:
- the wrapper for the JNI interface to use liboqs for Android [liboqs-android](https://github.com/Radotornado/Post-Quantum-digital-signature-app/tree/master/liboqs-android), which is essentially [open-quantum-safe/liboqs-java](https://github.com/open-quantum-safe/liboqs-java) with some minor modifications (mostly package changes and a different loading of liboqs.so). The prebuild liboqs.so files are generated with [build-android.sh](https://github.com/open-quantum-safe/liboqs/blob/main/scripts/build-android.sh) manually;
- the main application, consisting of a single main activity and XML layout.

## How to use
Upon opening the application, you are greeted with a dropdown with all supported algorithms. They are filtered to only show the [NIST finalists](https://csrc.nist.gov/projects/post-quantum-cryptography/round-3-submissions) in the third round. When an algorithm is chosen lower is given the following info about it:
- The full name of the signature scheme;
- The version (last git commit);
- The NIST security level (1, 2, 3, 4, 5) claimed in this algorithm's original NIST submission;
- Whether the signature offers EUF-CMA security (true) or not (false);
- The (maximum) length, in bytes, of public keys for this signature scheme;
- The (maximum) length, in bytes, of secret keys for this signature scheme;
- The (maximum) length, in bytes, of signatures for this signature scheme.

Further down there is a text field with example, changeable text, which serves as the message to be signed. Under it there is the signature, represented in ASCII characters. 
The main controls are under the signed message: 
- The first button ```Validate exact``` validates the unmodified signature, using the message, signature and public key;
- The second button ```Show/Hide public key``` shows and hides the generated public key (keep in mind that the public key for Rainbow is usually very big and showing/calculating it will slow your phone down);
- The third button ```Shuffle signature and validate``` randomly shuffles the bits inside the signature and tries to validate it. An appropriate message is thrown. This simulates a scenario, when certificate of the right length is given, but it is not correct.
- The last button ```Alter signature length and validate``` adds one extra bit at the end of the signature. As this is not accepted it triggers a handled *RuntimeException* and an appropriate message is shown.

## Screenshots
<p float="left" align="middle">
<img src="/screenshots/01.jpg" width="300" alt="Example with Falcon-512 and valid signature" style="margin-right:10px"/>
<img src="/screenshots/02.jpg" width="300" alt="Example with Rainbow-Vc-Cyclic-Compressed and altered signature"style="margin-left:10px"/>
</p>
