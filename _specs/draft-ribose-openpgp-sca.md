---
title: SCA Extensions For OpenPGP
description: >-
  Enables OpenPGP to be used in compliance
  with SCA regulations within China.

feature_with_priority: 2

external_links:
  - url: https://datatracker.ietf.org/doc/draft-ribose-openpgp-sca
  - url: https://github.com/riboseinc/rfc-openpgp-sca

tags: [Draft]
---

```
Internet Research Task Force                                      R. Tse
Internet-Draft                                                    Ribose
Updates: 4880, 6637 (if approved)                                W. Wong
Intended status: Standards Track            Hang Seng Management College
Expires: June 18, 2018                                          J. Lloyd
                                                                D. Wyatt
                                                             E. Borsboom
                                                                  Ribose
                                                       December 15, 2017

                       SCA Extensions For OpenPGP
                      draft-ribose-openpgp-sca-00

Abstract

   This document enables OpenPGP (RFC4880) to be used in a compliant
   manner according to regulations set by the SCA (the State
   Cryptography Administration of China) within China.

   Specifically, it extends OpenPGP to support the usage of SM2, SM3 and
   SM4 algorithms, and provides the SCA-compliant OpenPGP profile "SCA-
   SM234".

Status of This Memo

   This Internet-Draft is submitted in full conformance with the
   provisions of BCP 78 and BCP 79.

   Internet-Drafts are working documents of the Internet Engineering
   Task Force (IETF).  Note that other groups may also distribute
   working documents as Internet-Drafts.  The list of current Internet-
   Drafts is at https://datatracker.ietf.org/drafts/current/.

   Internet-Drafts are draft documents valid for a maximum of six months
   and may be updated, replaced, or obsoleted by other documents at any
   time.  It is inappropriate to use Internet-Drafts as reference
   material or to cite them other than as "work in progress."

   This Internet-Draft will expire on June 18, 2018.

Copyright Notice

   Copyright (c) 2017 IETF Trust and the persons identified as the
   document authors.  All rights reserved.

   This document is subject to BCP 78 and the IETF Trust's Legal
   Provisions Relating to IETF Documents

Tse, et al.               Expires June 18, 2018                 [Page 1]
Internet-Draft         SCA Extensions for OpenPGP          December 2017

   (https://trustee.ietf.org/license-info) in effect on the date of
   publication of this document.  Please review these documents
   carefully, as they describe your rights and restrictions with respect
   to this document.

Table of Contents

   1.  Introduction  . . . . . . . . . . . . . . . . . . . . . . . .   3
   2.  Terms and Definitions . . . . . . . . . . . . . . . . . . . .   4
   3.  Symbols And Abbreviations . . . . . . . . . . . . . . . . . .   4
   4.  SM2 Algorithms  . . . . . . . . . . . . . . . . . . . . . . .   5
     4.1.  SM2 Digital Signature Algorithm . . . . . . . . . . . . .   6
     4.2.  SM2 Key Exchange Protocol . . . . . . . . . . . . . . . .   7
     4.3.  SM2 Public Key Encryption . . . . . . . . . . . . . . . .   7
     4.4.  Recommended SM2 Curve . . . . . . . . . . . . . . . . . .   7
       4.4.1.  Definitions . . . . . . . . . . . . . . . . . . . . .   8
       4.4.2.  Elliptic Curve Formula  . . . . . . . . . . . . . . .   8
       4.4.3.  Curve Parameters  . . . . . . . . . . . . . . . . . .   8
     4.5.  Data Formats  . . . . . . . . . . . . . . . . . . . . . .   8
       4.5.1.  Secret Key Data Format  . . . . . . . . . . . . . . .   8
       4.5.2.  Encrypted Data Format . . . . . . . . . . . . . . . .   9
       4.5.3.  Signature Data Format . . . . . . . . . . . . . . . .   9
   5.  SM3 Hash Algorithm  . . . . . . . . . . . . . . . . . . . . .  10
   6.  SM4 Symmetric Encryption Algorithm  . . . . . . . . . . . . .  10
   7.  Supported Algorithms  . . . . . . . . . . . . . . . . . . . .  11
     7.1.  Public Key Algorithms . . . . . . . . . . . . . . . . . .  11
     7.2.  Symmetric Key Algorithms  . . . . . . . . . . . . . . . .  11
     7.3.  Hash Algorithms . . . . . . . . . . . . . . . . . . . . .  12
   8.  Conversion Primitives . . . . . . . . . . . . . . . . . . . .  12
   9.  SM2 Key Derivation Function . . . . . . . . . . . . . . . . .  12
     9.1.  Prerequisites . . . . . . . . . . . . . . . . . . . . . .  13
     9.2.  Inputs  . . . . . . . . . . . . . . . . . . . . . . . . .  13
     9.3.  Outputs . . . . . . . . . . . . . . . . . . . . . . . . .  13
   10. Encoding of Public and Private Keys . . . . . . . . . . . . .  14
     10.1.  Public-Key Packet Formats  . . . . . . . . . . . . . . .  14
     10.2.  Secret-Key Packet Formats  . . . . . . . . . . . . . . .  15
   11. Message Encoding with Public Keys . . . . . . . . . . . . . .  15
     11.1.  Public-Key Encrypted Session Key Packets (Tag 1) . . . .  15
     11.2.  Signature Packet (Tag 2) . . . . . . . . . . . . . . . .  16
       11.2.1.  Version 3 Signature Packet Format  . . . . . . . . .  16
       11.2.2.  Version 4 Signature Packet Format  . . . . . . . . .  16
   12. SM2 ECC Curve OID . . . . . . . . . . . . . . . . . . . . . .  16
   13. Compatibility Profiles  . . . . . . . . . . . . . . . . . . .  17
     13.1.  SCA SM234 Profile  . . . . . . . . . . . . . . . . . . .  17
   14. Security Considerations . . . . . . . . . . . . . . . . . . .  17
   15. IANA Considerations . . . . . . . . . . . . . . . . . . . . .  18
   16. References  . . . . . . . . . . . . . . . . . . . . . . . . .  18
     16.1.  Normative References . . . . . . . . . . . . . . . . . .  18

Tse, et al.               Expires June 18, 2018                 [Page 2]
Internet-Draft         SCA Extensions for OpenPGP          December 2017

     16.2.  Informative References . . . . . . . . . . . . . . . . .  20
   Appendix A.  Examples . . . . . . . . . . . . . . . . . . . . . .  25
     A.1.  Public Key Example  . . . . . . . . . . . . . . . . . . .  25
     A.2.  Signature Example . . . . . . . . . . . . . . . . . . . .  25
   Appendix B.  Acknowledgements . . . . . . . . . . . . . . . . . .  26
   Authors' Addresses  . . . . . . . . . . . . . . . . . . . . . . .  26

1.  Introduction

   SM2 [GBT.32918.1-2016] [I-D.shen-sm2-ecdsa], SM3 [GBT.32905-2016]
   [I-D.sca-cfrg-sm3] and kM4 [GBT.32907-2016] [I-D.ribose-cfrg-sm4] are
   cryptographic standards issued by the State Cryptography
   Administration [SCA] (formerly OSCCA, the Office of State Commercial
   Cryptography Administration of China) as authorized cryptographic
   algorithms for use within China.  These algorithms are published in
   public.

   Adoption of this document enables exchange of OpenPGP-secured email
   [RFC4880] in a SCA-compliant manner through usage of the authorized
   combination of SM2, SM3 and SM4.

   SM2 is an elliptic curve cryptosystem (ECC) that is composed of a set
   of public key cryptographic algorithms based on elliptic curves and
   also a recommended elliptic curve:

   o  Digital Signature Algorithm [GBT.32918.2-2016]

   o  Key Exchange Protocol [GBT.32918.3-2016]

   o  Public Key Encryption Algorithm [GBT.32918.4-2016]

   o  SM2 Recommended Elliptic Curve [GBT.32918.5-2017]

   SM3 [GBT.32905-2016] is a hash algorithm designed for electronic
   authentication purposes.

   SM4 [GBT.32907-2016] is a symmetric encryption algorithm designed for
   data encryption.

   SM2, SM3 and SM4 are standardized at ISO as [ISO.IEC.14888-3],
   [ISO.IEC.10118-3], and [ISO.IEC.18033-3.AMD2] respectively.

   This document extends OpenPGP [RFC4880] and its ECC extension
   [RFC6637] to support SM2, SM3 and SM4:

   o  support the SM3 hash algorithm for data validation purposes

Tse, et al.               Expires June 18, 2018                 [Page 3]
Internet-Draft         SCA Extensions for OpenPGP          December 2017

   o  support signatures utilizing the combination of SM3 with other
      digital signing algorithms, such as RSA, ECDSA and SM2

   o  support the SM2 asymmetric encryption algorithm for public key
      operations

   o  support usage of SM2 in combination with supported hash
      algorithms, such as SHA-256 and SM3

   o  support the SM4 symmetric encryption algorithm for data protection
      purposes

   o  defines the OpenPGP profile "SCA-SM234" to enable usage of OpenPGP
      in an SCA-compliant manner.

2.  Terms and Definitions

   The key words "*MUST*", "*MUST NOT*", "*REQUIRED*", "*SHALL*",
   "*SHALL NOT*", "*SHOULD*", "*SHOULD NOT*", "*RECOMMENDED*", "*MAY*",
   and "*OPTIONAL*" in this document are to be interpreted as described
   in [RFC2119].

   Compliant applications are a subset of the broader set of OpenPGP
   applications described in [RFC4880].  Any [RFC2119] keyword within
   this document applies to compliant applications only.

   The following terms and definitions apply to this document.

   SCA-compliant
      All cryptographic algorithms used are compliant with SCA [SCA]
      regulations.

   SM2DSA
      The elliptic curve digital signature algorithm defined in
      [GBT.32918.2-2016]

   SM2KEP
      The elliptic curve key exchange protocol defined in
      [GBT.32918.3-2016]

   SM2PKE
      The public key encryption algorithm defined in [GBT.32918.4-2016]

3.  Symbols And Abbreviations

   This document utilizes definitions of operations from [RFC7253] and
   are included here for reference.

Tse, et al.               Expires June 18, 2018                 [Page 4]
Internet-Draft         SCA Extensions for OpenPGP          December 2017

   c^i
      The integer c raised to the i-th power.

   S || T
      String S concatenated with string T (e.g., 000 || 111 == 000111).

4.  SM2 Algorithms

   SM2 is an elliptic curve based cryptosystem (ECC) [GBT.32918.1-2016]
   [I-D.shen-sm2-ecdsa] published by [SCA].

   It was first published by the SCA ("OSCCA" at that time) in public in
   2010 [OSCCA-SM2], then standardized as [GMT-0003-2012] in 2012,
   included in [ISO.IEC.11889] in 2015, published as a Chinese National
   Standard as [GBT.32918.1-2016], and published in [ISO.IEC.14888-3] in
   2017.

   The SM2 cryptosystem [I-D.shen-sm2-ecdsa] is published in 5 parts,
   covering:

   o  Part 1: General [GBT.32918.1-2016]

   o  Part 2: Digital Signature Algorithm [GBT.32918.2-2016]

   o  Part 3: Key Exchange [GBT.32918.3-2016]

   o  Part 4: Public Key Encryption Algorithm [GBT.32918.4-2016]

   o  Part 5: Parameter Definition [GBT.32918.5-2017]

   Specifically, it is composed of three distinct algorithms:

   o  an elliptical curve digital signature algorithm ("SM2DSA")
      [GBT.32918.2-2016] [ISO.IEC.14888-3] [SM2-2]

   o  a key exchange protocol ("SM2KEP") [GBT.32918.3-2016]; and

   o  a public key encryption algorithm ("SM2PKE") [GBT.32918.4-2016].

   This document refers to the SM2DSA and SM2PKE algorithms for the
   usage of OpenPGP [RFC4880].

   [GMT-0009-2012] provides specifications on interoperable usage of SM2
   data formats, and they are adhered to within within this document.

Tse, et al.               Expires June 18, 2018                 [Page 5]
Internet-Draft         SCA Extensions for OpenPGP          December 2017

4.1.  SM2 Digital Signature Algorithm

   The SM2 Digital Signature Algorithm is intended for digital signature
   and verifications in commercial cryptographic applications,
   including, but not limited to:

   o  identity authentication

   o  protection of data integrity

   o  verification of data authenticity

   The process of digital signature signing and verification along with
   their examples are found in [GBT.32918.2-2016], [ISO.IEC.14888-3],
   [SM2-2], and also described in [I-D.shen-sm2-ecdsa].

   The SM2DSA process requires usage of a hash function within.  For
   SCA-compliant usage, a SCA-compliant hash function such as SM3
   [GBT.32905-2016] *MUST* also be used.

   Formal security proofs for SM2 are provided in [SM2-SigSecurity]
   indicating that it satisfies both EUF-CMA security and security
   against generalized strong key substitution attacks.

   The SM2DSA algorithm has been cryptanalyzed by multiple parties with
   the current strongest attack being nonce [SM2-DSA-Nonces]
   [SM2-DSA-Nonces2] and lattice attacks [SM2-DSA-Lattice].

   In terms of OpenPGP usage, SM2DSA is an alternative to the ECDSA
   algorithm specified in [RFC6637].

   For OpenPGP compatibility, these additional requirements *MUST* be
   adhered to:

   o  SM2DSA allows use of an optional "user identity" string which is
      hashed into "ZA" (Section 3.5 of [SM2-2] and Section 5.1.4.4 of
      [I-D.shen-sm2-ecdsa]).  In OpenPGP, the user identifier "IDA"
      *MUST* be the empty string.

   o  While SM2DSA usually signs "H(ZA || msg)" (Section 4.1 of
      [SM2-2]), this document follows the OpenPGP convention of
      [RFC6637] of not directly signing the raw message "msg", but its
      hash "H(msg)".  Therefore when a message is signed by SM2DSA in
      OpenPGP, the algorithm *MUST* sign the content of "H(ZA ||
      H(msg))" instead of "H(ZA || msg)".  The hash algorithm used here
      *MUST* be identical.

Tse, et al.               Expires June 18, 2018                 [Page 6]
Internet-Draft         SCA Extensions for OpenPGP          December 2017

4.2.  SM2 Key Exchange Protocol

   The SM2 Key Exchange Protocol is used for cryptographic key exchange,
   allowing the negotiation and exchange of a session key within two to
   three message transfers.

   The process of key exchange and verification along with their
   examples are found in [GBT.32918.3-2016] [SM2-3], and also described
   in [I-D.shen-sm2-ecdsa].

   SM2KEP is not used with OpenPGP as it is a two- to three- pass key
   exchange mechanism, while in OpenPGP, public keys of recipients are
   available initially.

   The SM2KEP is now considered insecure due to [SM2-KEP-Comments],
   similar in status to the Unified Model and MQV schemes described in
   [NIST.SP.800-56Ar2].

4.3.  SM2 Public Key Encryption

   The SM2 Public Key Encryption algorithm is an elliptic curve based
   asymmetric encryption algorithm.  It is used for cryptographic
   encryption and decryption, allowing the message sender to utilize the
   public key of the message receiver to encrypt the message, with the
   recipient decrypting the messaging using his private key.

   The full description of SM2PKE is provided in [GBT.32918.4-2016].

   It utilizes a public key size of 512 bits and private key size of 256
   bits [GBT.32918.4-2016] [GMT-0003-2012].

   The process of encryption and decryption, along with their examples
   are found in [GBT.32918.4-2016] and [SM2-4].

   The SM2PKE process requires usage of a hash function within.  For
   SCA-compliant usage, a SCA-compliant hash function such as SM3
   [GBT.32905-2016] *MUST* also be used.

   In OpenPGP, SM2PKE is an alternative to RSA specified in [RFC4880].

4.4.  Recommended SM2 Curve

   The recommended curve is specified in [GBT.32918.5-2017] [SM2-5] and
   provided here for reference.  SM2 uses a 256-bit elliptic curve.

Tse, et al.               Expires June 18, 2018                 [Page 7]
Internet-Draft         SCA Extensions for OpenPGP          December 2017

4.4.1.  Definitions

   p
      an integer larger than 3

   a, b
      elements of F_q, defines an elliptic curve E on F_q

   n
      Order of base point G (n is a prime factor of E(F_q))

   x_G
      x-coordinate of generator G

   y_G
      y-coordinate of generator G

4.4.2.  Elliptic Curve Formula

   y^2 = x^3 + ax + b

4.4.3.  Curve Parameters

   p   = FFFFFFFE FFFFFFFF FFFFFFFF FFFFFFFF
         FFFFFFFF 00000000 FFFFFFFF FFFFFFFF
   a   = FFFFFFFE FFFFFFFF FFFFFFFF FFFFFFFF
         FFFFFFFF 00000000 FFFFFFFF FFFFFFFC
   b   = 28E9FA9E 9D9F5E34 4D5A9E4B CF6509A7
         F39789F5 15AB8F92 DDBCBD41 4D940E93
   n   = FFFFFFFE FFFFFFFF FFFFFFFF FFFFFFFF
         7203DF6B 21C6052B 53BBF409 39D54123
   x_G = 32C4AE2C 1F198119 5F990446 6A39C994
         8FE30BBF F2660BE1 715A4589 334C74C7
   y_G = BC3736A2 F4F6779C 59BDCEE3 6B692153
         D0A9877C C62A4740 02DF32E5 2139F0A0

4.5.  Data Formats

   [GMT-0009-2012] defines a number of data formats for the SM2
   algorithm to allow interoperable implementations.  This document
   adheres to these conventions.

4.5.1.  Secret Key Data Format

   SM2 secret key data format is described in ASN.1 as [GMT-0009-2012]:

   SM2PrivateKey ::= INTEGER

Tse, et al.               Expires June 18, 2018                 [Page 8]
Internet-Draft         SCA Extensions for OpenPGP          December 2017

   SM2 public key data format is described in ASN.1 as [GMT-0009-2012]:

   SM2PublicKey ::= BIT STRING

   Where:

   o  "SM2PublicKey" is of type "BIT STRING" and with content "04 ||
      X || Y".

      *  "X" and "Y" specifies the x- and y-coordinates of the public
         key, each of 256-bits long.

4.5.2.  Encrypted Data Format

   The SM2 encrypted data format is provided by [GMT-0009-2012] as the
   following in ASN.1 format:

   SM2Cipher ::= SEQENCE{
     XCoordinate     INTEGER,                -- x-coordinate
     YCoordinate     INTEGER,                -- y-coordinate
     HASH            OCTET STRING SIZE(32),  -- hash value
     CipherText      OCTET STRING            -- ciphertext
   }

   Where:

   o  "XCoordinate" and "YCoordinate" are x- and y-coordinates on the
      elliptic curve, both 256 bits long.

   o  "HASH" is the hash value calculated from the hash function used in
      "KDF" of a fixed bit length of 256-bits.

   o  "CipherText" is of same length as its plaintext.

4.5.3.  Signature Data Format

   SM2 signature data format is described in ASN.1 as [GMT-0009-2012]:

   SM2Signature ::= SEQUENCE{
     R   INTEGER,  -- first portion of signature
     S   INTEGER   -- second portion of signature
   }

   "R" and "S" represent the first and second portion of the signature,
   and both are 256 bits long.

Tse, et al.               Expires June 18, 2018                 [Page 9]
Internet-Draft         SCA Extensions for OpenPGP          December 2017

5.  SM3 Hash Algorithm

   The SM3 Cryptographic Hash Algorithm [GBT.32905-2016] is an iterative
   hash function designed by Xiaoyun Wang et al., published by [SCA] as
   an alternative to SHA-2 [NIST.FIPS.180-4].

   The specification, security considerations and cryptanalysis results
   of SM3 are thoroughly presented in [I-D.sca-cfrg-sm3].

   It was first published by the SCA ("OSCCA" at that time) in public in
   2010 [SM3], then published as an industry cryptogrpahic standard in
   2012 [GMT-0004-2012], published as a Chinese National Standard in
   2016 as [GBT.32905-2016], and included in the [ISO.IEC.10118-3]
   standard in 2017.

   The algorithm is designed to be used for commercial cryptographic
   applications including, but not limited to:

   o  digital signatures and their verification

   o  message authentication code generation and their verification

   o  generation of random numbers

   SM3 has a Merkle-Damgard construction and is similar to SHA-2
   [NIST.FIPS.180-4] of the MD4 [RFC6150] family, with the addition of
   several strengthening features including a more complex step function
   and stronger message dependency than SHA-256 [SM3-Boomerang].

   SM3 produces an output hash value of 256 bits long, based on 512-bit
   input message blocks [GBT.32905-2016], on input lengths up to 2^(m).

6.  SM4 Symmetric Encryption Algorithm

   SM4 [GBT.32907-2016] is a symmetric encryption algorithm designed by
   Shuwang Lu et al. originally intended for the usage of wireless local
   area network (Wireless LAN) products.

   The specification, security considerations and cryptanalysis results
   of SM4 are thoroughly presented in [I-D.ribose-cfrg-sm4] .

   SM4 is a 128-bit blockcipher, uses a key size of 128 bits and
   internally uses an 8-bit S-box.  It performs 32 rounds per block.
   Decryption is achieved by reversing the order of encryption.

   SMS4 was first published in public as part of WAPI (Wired
   Authentication and Privacy Infrastructure), the Chinese National
   Standard for Wireless LAN [GB.15629.11-2003].  It was then published

Tse, et al.               Expires June 18, 2018                [Page 10]
Internet-Draft         SCA Extensions for OpenPGP          December 2017

   independently by SCA ("OSCCA" at that time) in 2006 [SM4], formally
   renamed to SM4 in 2012 [GMT-0002-2012], published as a Chinese
   National Standard in 2016 [GBT.32907-2016], and included in
   [ISO.IEC.18033-3.AMD2] in 2017.

   It is a required encryption algorithm specified in WAPI
   [GB.15629.11-2003].

7.  Supported Algorithms

7.1.  Public Key Algorithms

   The SM2 algorithm is supported with the following extension.

   The following public key algorithm IDs are added to expand
   Section 9.1 of [RFC4880], "Public-Key Algorithms":

                    +-----+--------------------------+
                    | ID  | Description of Algorithm |
                    +-----+--------------------------+
                    | TBD | SM2                      |
                    +-----+--------------------------+

   Compliant applications *MUST* support both usages of SM2 Section 4:

   o  SM2 Digital Signature Algorithm (SM2DSA) [GBT.32918.2-2016]

   o  SM2 Public Key Encryption (SM2PKE) [GBT.32918.4-2016]

7.2.  Symmetric Key Algorithms

   The SM4 algorithm is supported with the following extension.

   The following symmetric encryption algorithm ID is added to expand
   Section 9.2 of [RFC4880], "Symmetric-Key Algorithms":

                    +-----+--------------------------+
                    | ID  | Description of Algorithm |
                    +-----+--------------------------+
                    | TBD | SM4                      |
                    +-----+--------------------------+

   Compliant applications *MUST* support SM4 Section 6.

Tse, et al.               Expires June 18, 2018                [Page 11]
Internet-Draft         SCA Extensions for OpenPGP          December 2017

7.3.  Hash Algorithms

   The SM3 algorithm is supported with the following extension.

   The following symmetric encryption algorithm IDs are added to expand
   Section 9.3 of [RFC4880], "Hash Algorithms":

                    +-----+--------------------------+
                    | ID  | Description of Algorithm |
                    +-----+--------------------------+
                    | TBD | SM3                      |
                    +-----+--------------------------+

   Compliant applications *MUST* support SM3 Section 5.

8.  Conversion Primitives

   The encoding method of [RFC6637] Section 6 *MUST* be used, and is
   compatible with the definition given in [SEC1].

   For clarity, according to the EC curve MPI encoding method of
   [RFC6637], the exact size of the MPI payload for the "SM2
   Recommended" 256-bit curve [GBT.32918.5-2017], is 515 bits.

9.  SM2 Key Derivation Function

   A key derivation function (KDF) is necessary to implement EC
   encryption.

   The SM2PKE KDF is defined in Section 3.4.3 of [GBT.32918.4-2016] (and
   Section 5.4.3 of [I-D.shen-sm2-ecdsa], Section 3.4.3 of [SM2-4]).

   For SCA-compliance, it *SHOULD* be used in conjunction with an SCA-
   approved hash algorithm, such as SM3 [GBT.32905-2016].

   The SM2PKE KDF is equivalent to the KDF2 function defined in
   Section 13.2 of [IEEE.1363a.2004] given the following assignments:

   o  Parameter

      *  v as hBits, the output length of the selected hash function
         Hash

   o  Input

      *  KEYLEN as oBits

      *  Z as the plaintext string; and

Tse, et al.               Expires June 18, 2018                [Page 12]
Internet-Draft         SCA Extensions for OpenPGP          December 2017

      *  PB is set to the empty bit string.

   Pseudocode of the SM2KDF function is provided here for convenience.
   This function contains edited variable names for clarity.

9.1.  Prerequisites

   o  Hash(S) is a hash function that outputs a v-bit long hash value
      based on input S.

   o  MSB(b, S) is a function that outputs the b most significant bits
      of the bitstream S.

   o  Floor(r) and Ceil(r) are the floor and ceiling functions
      respectively for the input of real number r.  Both functions
      outputs an integer.

9.2.  Inputs

   KEYLEN
      Desired key length.  A positive integer less than (2^32 - 1) x v.

   Z
      Plaintext.  String of any length.

9.3.  Outputs

   K
      Generated key.  String of length KEYLEN.

   K is defined as follows.

Tse, et al.               Expires June 18, 2018                [Page 13]
Internet-Draft         SCA Extensions for OpenPGP          December 2017

   _____________________________________________________________________

   Counter = 1                  // a 32-bit counter
   n = KEYLEN / v

   for each 1 <= i <= Ceil(n)
     Ha_i = Hash( Z || Counter )
     Counter = Counter + 1
   end for

   if n is a whole number then
     Ha! = Ha_{Ceil(n)}
   else
     Ha! = MSB(KEYLEN - (v x Floor(n)), Ha_{Ceil(n)})
   end if

   K = Ha_1 || Ha_2 || ... || Ha_{Ceil(n)-1} || Ha!
   _____________________________________________________________________

10.  Encoding of Public and Private Keys

10.1.  Public-Key Packet Formats

   The following algorithm-specific packets are added to Section 5.5.2
   of [RFC4880], "Public-Key Packet Formats", to support SM2DSA and
   SM2PKE.

   This document extends the algorithm-specific portion with the
   following fields.

   Algorithm-Specific Fields for SM2DSA keys:

   o  a variable-length field containing a curve OID, formatted as
      follows:

      *  a one-octet size of the following field; values 0 and 0xFF are
         reserved for future extensions

      *  octets representing a curve OID, described in Section 12

   o  MPI of an EC point representing a public key

   Algorithm-Specific Fields for SM2PKE keys:

   o  a variable-length field containing a curve OID, formatted as
      follows:

Tse, et al.               Expires June 18, 2018                [Page 14]
Internet-Draft         SCA Extensions for OpenPGP          December 2017

      *  a one-octet size of the following field; values 0 and 0xFF are
         reserved for future extensions

      *  octets representing a curve OID, described in Section 12

   o  MPI of an EC point representing a public key

   Note that both SM2DSA and SM2PKE public keys are composed of the same
   sequence of fields, and use the same codepoint to identify them.
   They are distinguished by the key usage flags.

10.2.  Secret-Key Packet Formats

   The following algorithm-specific packets are added to Section 5.5.3.
   of [RFC4880], "Secret-Key Packet Formats", to support SM2DSA and
   SM2PKE.

   This document extends the algorithm-specific portion with the
   following fields.

   Algorithm-Specific Fields for SM2DSA or SM2PKE secret keys:

   o  an MPI of an integer representing the secret key, which is a
      scalar of the public EC point

11.  Message Encoding with Public Keys

11.1.  Public-Key Encrypted Session Key Packets (Tag 1)

   Section 5.1 of [RFC4880], "Public-Key Encrypted Session Key Packets
   (Tag 1)" is extended to support SM2PKE using the following algorithm
   specific fields for SM2PKE, through applying the KDF described in
   Section 9.

   Algorithm Specific Fields for SM2 encryption:

   o  The SM2 ciphertext is formatted in the OpenPGP bitstream as a
      single MPI.  This consists of:

      *  The data format described in Section 4.5.2 containing data
         provided by [GBT.32918.4-2016] Section 6.1 step A8 ("C = (C1 ||
         C3 || C2)"), followed by

      *  a single octet giving the code for the hash algorithm used
         within the calculation of the KDF mask "t" (step A5 of
         [GBT.32918.4-2016] Section 6.1) and the calculation of "C3"
         (step A7 of [GBT.32918.4-2016] Section 6.1).  For SCA
         compliance, this *MUST* be an SCA-approved hash function, and

Tse, et al.               Expires June 18, 2018                [Page 15]
Internet-Draft         SCA Extensions for OpenPGP          December 2017

         in any case, it *SHOULD* be a hash which is listed in the
         receiving keys "Preferred Hash Algorithms" list
         (Section 5.2.3.8 of [RFC4880]).

11.2.  Signature Packet (Tag 2)

11.2.1.  Version 3 Signature Packet Format

   Section 5.2.2 of [RFC4880] defines the signature format for "Version
   3 Signature Packet Format".  Similar to ECDSA [RFC6637], no change in
   the format is necessary for SM2DSA.

11.2.2.  Version 4 Signature Packet Format

   Section 5.2.3 of [RFC4880] defines the signature format for "Version
   4 Signature Packet Format".  Similar to ECDSA [RFC6637], no change in
   the format is necessary for SM2DSA.

12.  SM2 ECC Curve OID

   This section provides the curve ASN.1 Object Identifier (OID) of the
   "SM2 Recommended Curve" [GBT.32918.5-2017] described in Section 4,
   according to the method of [RFC6637].

   We specify the curve OID of the "SM2 Recommended Curve" to be the
   registered OID entry of "SM2 Elliptic Curve Cryptography" according
   to [GMT-0006-2012], which is "1.2.156.10197.1.301".

   The table below specifies the exact sequence of bytes of the
   mentioned curve:

   +---------------------+--------+--------------------+---------------+
   | ASN.1 OID           | OID    | Curve OID bytes in | Curve name    |
   |                     | len    | hex                |               |
   +---------------------+--------+--------------------+---------------+
   | 1.2.156.10197.1.301 | 8      | 2A 81 1C CF 55 01  | SM2           |
   |                     |        | 82 2D              | Recommended   |
   +---------------------+--------+--------------------+---------------+

   The complete ASN.1 DER encoding for the SM2 Recommended curve OID is
   "06 08 2A 81 1C CF 55 01 82 2D", from which the first entry in the
   table above is constructed by omitting the first two octets.  Only
   the truncated sequence of octets is the valid representation of a
   curve OID.

Tse, et al.               Expires June 18, 2018                [Page 16]
Internet-Draft         SCA Extensions for OpenPGP          December 2017

13.  Compatibility Profiles

13.1.  SCA SM234 Profile

   The "SCA SM234" profile is designed to be compliant to SCA
   regulations.  A compliant OpenPGP implementation *MUST* implement the
   following items as described by this document:

   o  SM2 Recommended Curve (Section 12)

   o  SM2 (SM2DSA and SM2PKE) (Section 4)

      *  The hash function selected in SM2DSA and SM2PKE *MUST* also be
         SCA-compliant, such as SM3 [SM3]

   o  SM3 (Section 5)

   o  SM4 (Section 6)

14.  Security Considerations

   o  Products and services that utilize cryptography are regulated by
      the SCA [SCA]; they must be explicitly approved or certified by
      the SCA before being allowed to be sold or used in China.

   o  SM2 [GBT.32918.1-2016] is an elliptic curve cryptosystem (ECC)
      approved by the SCA [SCA].  Its security relies on the assumption
      that the elliptic curve discrete logarithm problem (ECLP) is
      computationally infeasible.  With advances in cryptanalysis, new
      attack algorithms may reduce the complexity of ECLP, making it
      easier to attack the SM2 cryptosystem that is considered secure at
      the time this document is published.  You *SHOULD* check current
      literature to determine if the algorithms in SM2 have been found
      vulnerable.

   o  There are security concerns with regards to side-channel attacks
      against ECCs, including template attacks (such as [SM2-Template])
      that rely on physical access to the computation device.  An
      implementer of ECC systems *SHOULD* be aware of potential
      vulnerabilities in this regard.

   o  SM3 [GBT.32905-2016] is a cryptographic hash algorithm approved by
      the SCA [SCA].  Security considerations provided in
      [I-D.sca-cfrg-sm3] apply.  There are no known practical attacks
      against the SM3 algorithm at the time this document is published.

   o  SM4 [GBT.32907-2016] is a blockcipher approved by the SCA [SCA].
      Security considerations of SM4 offered in [I-D.ribose-cfrg-sm4]

Tse, et al.               Expires June 18, 2018                [Page 17]
Internet-Draft         SCA Extensions for OpenPGP          December 2017

      apply.  No formal proof of security is provided but there are no
      known practical attacks against the SM4 algorithm by the time of
      publishing this document.
      There are security concerns with regards to side-channel attacks,
      when the SM4 algorithm is implemented in a device [SM4-Power].
      Side-channel security concerns are described in
      [I-D.ribose-cfrg-sm4].  When the SM4 algorithm is implemented in
      hardware, the parameters/keys *SHOULD* be randomly generated
      without fixed correlation.

   o  SM2 has a key length of 512 bits for the public key and 256 bits
      for the private key.  It is considered an alternative to ECDSA
      P-256 [RFC6637].  Its security strength is comparable to a 128-bit
      symmetric key strength [I-D.ietf-msec-mikey-ecc], e.g., AES-128
      [NIST.FIPS.197].

   o  SM3 is a hash function that generates a 256-bit hash value.  It is
      considered as an alternative to SHA-256 [RFC6234].

   o  SM4 is a blockcipher symmetric algorithm with a key length of 128
      bits.  It is considered as an alternative to AES-128
      [NIST.FIPS.197].

   o  Security considerations offered in [RFC6637] and [RFC4880] also
      apply.

15.  IANA Considerations

   The IANA "Pretty Good Privacy (PGP)" registry [RFC8126] has made the
   following assignments for algorithms described in this document,
   namely:

   o  ID XXX of the "Public Key Algorithms" namespace for SM2 Section 4

   o  ID XXX of the "Hash Algorithms" namespace for SM3 Section 5

   o  ID XXX of the "Symmetric Key Algorithms" namespace for SM4
      Section 6

16.  References

16.1.  Normative References

Tse, et al.               Expires June 18, 2018                [Page 18]
Internet-Draft         SCA Extensions for OpenPGP          December 2017

   [GBT.32905-2016]
              Standardization Administration of the People's Republic of
              China, "GB/T 32905-2016 Information Security Techniques --
              SM3 Cryptographic Hash Algorithm", August 2016,
              <http://www.gb688.cn/bzgk/gb/
              newGbInfo?hcno=45B1A67F20F3BF339211C391E9278F5E>.

   [GBT.32907-2016]
              Standardization Administration of the People's Republic of
              China, "GB/T 32907-2016 Information Security Technology --
              SM4 Block Cipher Algorithm", August 2016,
              <http://www.gb688.cn/bzgk/gb/
              newGbInfo?hcno=7803DE42D3BC5E80B0C3E5D8E873D56A>.

   [GBT.32918.2-2016]
              Standardization Administration of the People's Republic of
              China, "GB/T 32918.2-2016 Information Security Technology
              -- Public Key Cryptographic Algorithm SM2 Based On
              Elliptic Curves -- Part 2: Digital Signature Algorithm",
              August 2016, <http://www.gb688.cn/bzgk/gb/
              newGbInfo?hcno=6F1FAEB62F9668F25F38E0BF0291D4AC>.

   [GBT.32918.4-2016]
              Standardization Administration of the People's Republic of
              China, "GB/T 32918.4-2016 Information Security Technology
              -- Public Key Cryptographic Algorithm SM2 Based On
              Elliptic Curves -- Part 4: Public Key Encryption
              Algorithm", August 2016, <http://www.gb688.cn/bzgk/gb/
              newGbInfo?hcno=370AF152CB5CA4A377EB4D1B21DECAE0>.

   [GBT.32918.5-2017]
              Standardization Administration of the People's Republic of
              China, "GB/T 32918.5-2017 Information Security Technology
              -- Public Key Cryptographic Algorithm SM2 Based On
              Elliptic Curves -- Part 5: Parameter Definition", May
              2017, <http://www.gb688.cn/bzgk/gb/
              newGbInfo?hcno=728DEA8B8BB32ACFB6EF4BF449BC3077>.

   [RFC2119]  Bradner, S., "Key words for use in RFCs to Indicate
              Requirement Levels", BCP 14, RFC 2119,
              DOI 10.17487/RFC2119, March 1997,
              <https://www.rfc-editor.org/info/rfc2119>.

   [RFC4880]  Callas, J., Donnerhacke, L., Finney, H., Shaw, D., and R.
              Thayer, "OpenPGP Message Format", RFC 4880,
              DOI 10.17487/RFC4880, November 2007,
              <https://www.rfc-editor.org/info/rfc4880>.

Tse, et al.               Expires June 18, 2018                [Page 19]
Internet-Draft         SCA Extensions for OpenPGP          December 2017

   [RFC6637]  Jivsov, A., "Elliptic Curve Cryptography (ECC) in
              OpenPGP", RFC 6637, DOI 10.17487/RFC6637, June 2012,
              <https://www.rfc-editor.org/info/rfc6637>.

16.2.  Informative References

   [BOTAN]    Lloyd, J., "Botan: Crypto and TLS for C++11", October
              2017, <https://botan.randombit.net>.

   [GB.15629.11-2003]
              Standardization Administration of the People's Republic of
              China, "Information technology -- Telecommunications and
              information exchange between systems -- Local and
              metropolitan area networks -- Specific requirements --
              Part 11: Wireless LAN Medium Access Control (MAC) and
              Physical Layer (PHY) Specifications", May 2003,
              <http://www.gb688.cn/bzgk/gb/
              newGbInfo?hcno=74B9DD11287E72408C19C4D3A360D1BD>.

   [GBT.32918.1-2016]
              Standardization Administration of the People's Republic of
              China, "GB/T 32918.1-2016 Information Security Technology
              -- Public Key Cryptographic Algorithm SM2 Based On
              Elliptic Curves -- Part 1: General", August 2016,
              <http://www.gb688.cn/bzgk/gb/
              newGbInfo?hcno=3EE2FD47B962578070541ED468497C5B>.

   [GBT.32918.3-2016]
              Standardization Administration of the People's Republic of
              China, "GB/T 32918.3-2016 Information Security Technology
              -- Public Key Cryptographic Algorithm SM2 Based On
              Elliptic Curves -- Part 3: Key Exchange", August 2016,
              <http://www.gb688.cn/bzgk/gb/
              newGbInfo?hcno=66A89DD6DA64F49C49456B757BA0624F>.

   [GMT-0002-2012]
              Office of State Commercial Cryptography Administration of
              China, "GM/T 0002-2012: SM4 Block Cipher Algorithm", March
              2012, <http://www.oscca.gov.cn/Column/Column_32.htm>.

   [GMT-0003-2012]
              Office of State Commercial Cryptography Administration of
              China, "GM/T 0003-2012: Public Key Cryptographic Algorithm
              SM2 Based on Elliptic Curves", March 2012,
              <http://www.oscca.gov.cn/Column/Column_32.htm>.

Tse, et al.               Expires June 18, 2018                [Page 20]
Internet-Draft         SCA Extensions for OpenPGP          December 2017

   [GMT-0004-2012]
              Office of State Commercial Cryptography Administration of
              China, "GM/T 0004-2012: SM3 Hash Algorithm", March 2012,
              <http://www.oscca.gov.cn/Column/Column_32.htm>.

   [GMT-0006-2012]
              Office of State Commercial Cryptography Administration of
              China, "GM/T 0006-2012: Cryptographic Application
              Identifier Criterion Specification", March 2012,
              <http://www.oscca.gov.cn/Column/Column_32.htm>.

   [GMT-0009-2012]
              Office of State Commercial Cryptography Administration of
              China, "GM/T 0009-2012: SM2 cryptography algorithm
              application specification", March 2012,
              <http://www.oscca.gov.cn/Column/Column_32.htm>.

   [I-D.ietf-msec-mikey-ecc]
              Milne, A., "ECC Algorithms for MIKEY", draft-ietf-msec-
              mikey-ecc-03 (work in progress), June 2007.

   [I-D.ribose-cfrg-sm4]
              Tse, R. and W. Wong, "The SM4 Blockcipher Algorithm And
              Its Modes Of Operations", draft-ribose-cfrg-sm4-08 (work
              in progress), December 2017.

   [I-D.sca-cfrg-sm3]
              Shen, S., Lee, X., Tse, R., Wong, W., and P. Yang, "The
              SM3 Cryptographic Hash Function", draft-sca-cfrg-sm3-00
              (work in progress), December 2017.

   [I-D.shen-sm2-ecdsa]
              Shen, S., Shen, S., and X. Lee, "SM2 Digital Signature
              Algorithm", draft-shen-sm2-ecdsa-02 (work in progress),
              February 2014.

   [IEEE.1363a.2004]
              Institute of Electrical and Electronics Engineers, "IEEE
              Std 1363a-2004: IEEE Standard Specifications for Public-
              Key Cryptography -- Amendment 1: Additional Techniques",
              September 2004, <http://grouper.ieee.org/groups/1363/>.

   [ISO.IEC.10118-3]
              International Organization for Standardization, "ISO/IEC
              FDIS 10118-3 -- Information technology -- Security
              techniques -- Hash-functions -- Part 3: Dedicated hash-
              functions", September 2017,
              <https://www.iso.org/standard/67116.html>.

Tse, et al.               Expires June 18, 2018                [Page 21]
Internet-Draft         SCA Extensions for OpenPGP          December 2017

   [ISO.IEC.11889]
              International Organization for Standardization, "ISO/IEC
              11889-1:2015 -- Information technology -- Trusted platform
              module library", August 2015,
              <https://www.iso.org/standard/66510.html>.

   [ISO.IEC.14888-3]
              International Organization for Standardization, "ISO/IEC
              14888-3:2016-03 -- Information technology -- Security
              techniques -- Digital signatures with appendix -- Part 3:
              Discrete logarithm based mechanisms", September 2017,
              <https://www.iso.org/standard/70631.html>.

   [ISO.IEC.18033-3.AMD2]
              International Organization for Standardization, "ISO/IEC
              WD1 18033-3/AMD2 -- Information technology -- Security
              techniques -- Encryption algorithms -- Part 3: Block
              ciphers -- Amendment 2", June 2017,
              <https://www.iso.org/standard/54531.html>.

   [NIST.FIPS.180-4]
              National Institute of Standards and Technology, "FIPS
              180-4 Secure Hash Standard (SHS)", August 2015,
              <http://dx.doi.org/10.6028/NIST.FIPS.180-4>.

   [NIST.FIPS.197]
              National Institute of Standards and Technology, "FIPS 197
              Advanced Encryption Standard (AES)", November 2001,
              <https://doi.org/10.6028/NIST.FIPS.197>.

   [NIST.SP.800-56Ar2]
              Barker, B., Chen, L., Roginsky, A., and M. Smid, "SP
              800-56Ar2 Recommendation for Pair-Wise Key Establishment
              Schemes Using Discrete Logarithm Cryptography", May 2013,
              <http://dx.doi.org/10.6028/NIST.SP.800-56Ar2>.

   [OSCCA-SM2]
              Office of State Commercial Cryptography Administration of
              China, "Public Key Cryptographic Algorithm SM2 Based on
              Elliptic Curves", December 2010,
              <http://www.oscca.gov.cn/UpFile/2010122214822692.pdf>.

   [RFC6150]  Turner, S. and L. Chen, "MD4 to Historic Status",
              RFC 6150, DOI 10.17487/RFC6150, March 2011,
              <https://www.rfc-editor.org/info/rfc6150>.

Tse, et al.               Expires June 18, 2018                [Page 22]
Internet-Draft         SCA Extensions for OpenPGP          December 2017

   [RFC6234]  Eastlake 3rd, D. and T. Hansen, "US Secure Hash Algorithms
              (SHA and SHA-based HMAC and HKDF)", RFC 6234,
              DOI 10.17487/RFC6234, May 2011,
              <https://www.rfc-editor.org/info/rfc6234>.

   [RFC7253]  Krovetz, T. and P. Rogaway, "The OCB Authenticated-
              Encryption Algorithm", RFC 7253, DOI 10.17487/RFC7253, May
              2014, <https://www.rfc-editor.org/info/rfc7253>.

   [RFC8126]  Cotton, M., Leiba, B., and T. Narten, "Guidelines for
              Writing an IANA Considerations Section in RFCs", BCP 26,
              RFC 8126, DOI 10.17487/RFC8126, June 2017,
              <https://www.rfc-editor.org/info/rfc8126>.

   [RNP]      Ribose Inc., "Botan: Crypto and TLS for C++11", October
              2017, <https://open.ribose.com>.

   [SCA]      State Cryptography Administration of China, "State
              Cryptography Administration of China", Dec 2017,
              <http://www.sca.gov.cn>.

   [SEC1]     Standards for Efficient Cryptography Group, "SEC 1:
              Elliptic Curve Cryptography", September 2010,
              <http://www.secg.org/SEC1-Ver-1.0.pdf>.

   [SM2-1]    Office of State Commercial Cryptography Administration of
              China, "Public Key Cryptographic Algorithm SM2 Based on
              Elliptic Curves -- Part 1: General", December 2010,
              <http://www.oscca.gov.cn/UpFile/2010122214822692.pdf>.

   [SM2-2]    Office of State Commercial Cryptography Administration of
              China, "Public Key Cryptographic Algorithm SM2 Based on
              Elliptic Curves -- Part 2: Digital Signature Algorithm",
              December 2010,
              <http://www.oscca.gov.cn/UpFile/2010122214822692.pdf>.

   [SM2-3]    Office of State Commercial Cryptography Administration of
              China, "Public Key Cryptographic Algorithm SM2 Based on
              Elliptic Curves -- Part 3: Key Exchange Protocol",
              December 2010,
              <http://www.oscca.gov.cn/UpFile/2010122214822692.pdf>.

   [SM2-4]    Office of State Commercial Cryptography Administration of
              China, "Public Key Cryptographic Algorithm SM2 Based on
              Elliptic Curves -- Part 4: Public Key Encryption
              Algorithm", December 2010,
              <http://www.oscca.gov.cn/UpFile/2010122214822692.pdf>.

Tse, et al.               Expires June 18, 2018                [Page 23]
Internet-Draft         SCA Extensions for OpenPGP          December 2017

   [SM2-5]    Office of State Commercial Cryptography Administration of
              China, "Public Key Cryptographic Algorithm SM2 Based on
              Elliptic Curves -- Part 5: Parameter definitions",
              December 2010,
              <http://www.oscca.gov.cn/UpFile/2010122214836668.pdf>.

   [SM2-DSA-Lattice]
              Cao, W., Feng, J., Zhu, S., Chen, H., Wu, W., Han, X., and
              X. Zheng, "Practical Lattice-Based Fault Attack and
              Countermeasure on SM2 Signature Algorithm", November 2016,
              <https://doi.org/10.1007/978-3-319-29814-6_6>.

   [SM2-DSA-Nonces]
              Liu, M., Chen, J., and H. Li, "Partially Known Nonces and
              Fault Injection Attacks on SM2 Signature Algorithm",
              November 2013,
              <https://dx.doi.org/10.1007/978-3-319-12087-4_22>.

   [SM2-DSA-Nonces2]
              Chen, J., Liu, M., Shi, H., and H. Li, "Mind Your Nonces
              Moving: Template-Based Partially-Sharing Nonces Attack on
              SM2 Digital Signature Algorithm", November 2015,
              <https://doi.acm.org/10.1145/2714576.2714587>.

   [SM2-KEP-Comments]
              Xu, X. and D. Feng, "Comments on the SM2 Key Exchange
              Protocol", December 2011,
              <https://dx.doi.org/10.1007/978-3-642-25513-7_12>.

   [SM2-SigSecurity]
              Zhang, Z., Yang, K., Zhang, J., and C. Chen, "Security of
              the SM2 Signature Scheme Against Generalized Key
              Substitution Attacks", December 2015,
              <https://link.springer.com/
              chapter/10.1007/978-3-319-27152-1_7>.

   [SM2-Template]
              Zhang, Z., Wu, L., Mu, Z., and X. Zhang, "A Novel Template
              Attack on wNAF Algorithm of ECC", November 2014,
              <https://doi.org/10.1109/CIS.2014.66>.

   [SM3]      Office of State Commercial Cryptography Administration of
              China, "SM3 Cryptographic Hash Algorithm", December 2010,
              <http://www.oscca.gov.cn/UpFile/20101222141857786.pdf>.

Tse, et al.               Expires June 18, 2018                [Page 24]
Internet-Draft         SCA Extensions for OpenPGP          December 2017

   [SM3-Boomerang]
              Bai, D., Yu, H., Wang, G., and X. Wang, "Improved
              Boomerang Attacks on Round-Reduced SM3 and Keyed
              Permutation of BLAKE-256", April 2015,
              <https://doi.org/10.1049/iet-ifs.2013.0380>.

   [SM4]      Office of State Commercial Cryptography Administration of
              China, "SM4 block cipher algorithm", December 2010,
              <http://www.oscca.gov.cn/UpFile/200621016423197990.pdf>.

   [SM4-Power]
              Du, Z., Wu, Z., Wang, M., and J. Rao, "Improved chosen-
              plaintext power analysis attack against SM4 at the round-
              output", October 2015, <https://www.researchgate.net/
              publication/285470160_Improved_chosen-
              plaintext_power_analysis_attack_against_SM4_at_the_round-
              output>.

Appendix A.  Examples

A.1.  Public Key Example

   This example is generated using the OpenPGP implementation RNP [RNP],
   with the SM2 and SM3 implementations from Botan [BOTAN].

   -----BEGIN PGP PUBLIC KEY BLOCK-----
   xlIEWbGKWmMIKoEcz1UBgi0CAwQx5lUJNwGp01AB7YfAye0oMmyIPYe/cQPVwh8/7RCu
   ywZLMDDAM7qn6TNqTtdKW+7tLFhtOC4yzDVK8UjN/ccazSBTTTIgMjU2LWJpdCBrZXkg
   PGphY2tAbG9jYWxob3N0PsJ0BBNjaQAmBQJZsYpfAhsDBQsJCAcCBhUICQoLAgUWAgMB
   AAkQC/UcNw0bAZcAAJt5AP4oXvi3xl2RUwAvVjlzXtLL87g6x9cIBS7EB/cvAsw78AEA
   /Wt6qWlBVZ6TYiqNPt9An/4cjKyNpAv7S9u3neGXWUU=
   =RJ3C
   -----END PGP PUBLIC KEY BLOCK-----

A.2.  Signature Example

   This example is also created using RNP [RNP] and Botan [BOTAN].

   Detached signature of the string "SM2 example" using the above key:

   -----BEGIN PGP SIGNATURE-----
   wmQEAGMIABYFAlmxj+cFAwAAAAAJEAv1HDcNGwGXAAB+SQEAy5AHKgiRxgOogB/2sfge
   JaVoLgpxvDp9yIcaLfP++xkBAPGuZ1f9FjxVd5jlCGd1jFzAPpt8N2Lc3FQDqVjgJvV9
   =Xbbj
   -----END PGP SIGNATURE-----

Tse, et al.               Expires June 18, 2018                [Page 25]
Internet-Draft         SCA Extensions for OpenPGP          December 2017

Appendix B.  Acknowledgements

   The authors would like to thank the following persons for their
   valuable advice and input.

   o  The Ribose RNP team for their input and implementation

Authors' Addresses

   Ronald Henry Tse
   Ribose
   Suite 1111, 1 Pedder Street
   Central, Hong Kong
   Hong Kong

   Email: ronald.tse@ribose.com
   URI:   https://www.ribose.com

   Dr. Wai Kit Wong
   Hang Seng Management College
   Hang Shin Link, Siu Lek Yuen
   Shatin, Hong Kong
   Hong Kong

   Email: wongwk@hsmc.edu.hk
   URI:   https://www.hsmc.edu.hk

   Jack E. Lloyd
   Ribose
   United States of America

   Email: jack.lloyd@ribose.com
   URI:   https://www.ribose.com

   D. E. Wyatt
   Ribose
   608 W Cork St, Apt 2
   Winchester, VA
   United States of America

   Email: daniel.wyatt@ribose.com
   URI:   https://www.ribose.com

Tse, et al.               Expires June 18, 2018                [Page 26]
Internet-Draft         SCA Extensions for OpenPGP          December 2017

   Erick Borsboom
   Ribose
   Suite 1111, 1 Pedder Street
   Central, Hong Kong
   Hong Kong

   Email: erick.borsboom@ribose.com
   URI:   https://www.ribose.com

Tse, et al.               Expires June 18, 2018                [Page 27]
```
