## Overview
Write an Object Oriented Python program that implements the full DES algorithm.

The two commands below specify the exact command-line syntax for invoking encryption and decryption.

`python3 DES.py -e message.txt key.txt encrypted.txt`

`python3 DES.py -d encrypted.txt key.txt decrypted.txt`

- Encryption (indicated with the -e argument in line 1)
  perform DES encryption on the plaintext in message.txt using the key in key.txt, and write the ciphertext to a file called encrypted.txt

- Decryption (indicated with the -d argument in line 2)
  perform DES decryption on the ciphertext in encrypted.txt using the key in key.txt, and write the recovered plaintext to decrypted.txt

## Explaination

The goal of the code is to encrypt the given text or ppm file using DES. The class called DES has encrypt, decrypt, img_encrypt, substitute, get_encryption_key and get_round_keys as member functions. The encrypt function begins by generating round keys for the encryption process. To generate the round keys encrypt calls the helper function get_encryption_key which permutes the user input key to get the encryption key using the key_permutation_1 lookup list. The encryption key is then passed to the get_round_keys function which shifts and permutes the bits of the of the encryption key to get the 16 round keys. Next the encrypt function reads the input plain text file into a bitvector and beings looping through all the bits in the file in 64-bit chunks. Every 64-bit chunk is divided into 32-bit halves. The right 32 bits are expanded to 48 bits using the expansion_permutation lookup list and then XOR’d with a round key. This output is then made back into 32 bits using the substitute function which uses the s_boxes to find the corresponding 4 bits for every 6 bits in the output. Finally, the output is permuted using the p_box lookup list and XOR’d with the left 32 bits. The old left half becomes the new right half, and the computed right half becomes the new left half. This process is carries out 16 times for each chunk of 64 bits from the file. All the bits are then appended together and written to a text file in Hex.

The decrypt function begins by reading the cipher text file which is in HEX. The “more_to_read” method of a bitvector can only be used when the bitvector is initialized with a file. So, in order to be able to use this method the input HEX file is first converted to binary, and the temporary binary file is used to initialize the bit vector. The decrypt function carries out the exact same process as the encrypt function described above but uses the round keys in reverse order. Finally, the decrypted text is written to a file as ascii characters.

## Limitation

Block ciphers such as DES should not be used in electronic code book (ECB) mode to encrypt viewable media such as images. Overall patterns in the data may still be obvious since each block of data is encrypted independently. I demonstrate this by encrypting a ppm image and then inspecting the cipher text.
