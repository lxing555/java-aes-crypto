java-aes-crypto
===============

A simple Android class for encrypting &amp; decrypting strings, aiming to avoid [serious cryptographic errors](http://tozny.com/blog/encrypting-strings-in-android-lets-make-better-mistakes/) that most such classes suffer from. [Show me the code](https://github.com/tozny/java-aes-crypto/blob/master/aes-crypto/src/main/java/com/tozny/crypto/android/AesCbcWithIntegrity.java)

#Features

Here are the features of this class. We believe that these properties are consistent with what a lot of people are looking for when encrypting Strings in Android.

* *Works for strings*: It should encrypt arbitrary strings or byte arrays. This means it needs to effectively handle multiple blocks (CBC) and partial blocks (padding). It consistently serializes and deserializes ciphertext, IVs, and key material using base64 to make it easy to store.
* *Algorithm & Mode*: We chose: AES 128, CBC, and PKCS5 padding. We would have picked GCM for its built-in integrity checking, but that's only available since Android Jelly Bean.
* *IV Handling*: We securely generate a random IV before each encryption and provide a simple class to keep the IV and ciphertext together so they're easy to keep track of and store. We set the IV and then request it back from the Cipher class for compatibility across various Android versions.
* *Key generation*: Random key generation with the updated generation code recommended for Android. If you want password-based keys, we provide functions to salt and generate them.
* *Integrity*: Lots of people think AES has integrity checking built in. The thinking goes, "if it decrypts correctly, it was generated by the person with the private key". Actually, AES CBC allows an attacker to modify the messages. Therefore, we've also added integrity checking in the form of a SHA 256 hash.


#How to include in project?

###Copy and paste
It's a single very simple java class, [AesCbcWithIntegrity.java](https://github.com/tozny/java-aes-crypto/blob/master/aes-crypto/src/main/java/com/tozny/crypto/android/AesCbcWithIntegrity.java) that works across most or all versions of Android. The class should be easy to paste into an existing codebase.

###Android Library project
The library is in Android library project format so you can clone this project and add as a library module/project. 
	
###Maven Dependency
We've also published the library AAR file to Maven central for simple one line gradle dependency management.


```groovy
dependencies {
    compile 'com.tozny:aes-crypto:0.0.1'
}
```

#Examples

##Generate new key


```java
  AesCbcWithIntegrity.SecretKeys keys = AesCbcWithIntegrity.generateKey();
```


##Encrypt

```java
   AesCbcWithIntegrity.CipherTextIvMac cipherTextIvMac = AesCbcWithIntegrity.encrypt("some test", keys);
   //store or send to server
   String ciphertextString = cipherTextIvMac.toString();
```

##Decrypt

```java
  //Use the constructor to re-create the CipherTextIvMac class from the string:
  CipherTextIvMac cipherTextIvMac = new CipherTextIvMac (cipherTextString);
  String plainText = AesCbcWithIntegrity.decryptString(cipherTextIvMac, keys);
```  


#License 
The included MIT license is compatible with open source or commercial products. 
Tozny also offers custom support and licensing terms if your organization has different needs. Contact us at [info@tozny.com](mailto:info@tozny.com) for more details.


