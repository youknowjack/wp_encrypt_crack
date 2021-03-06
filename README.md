# wp_encrypt_crack

An encryption cracker for Word Perfect 4.2 (Originally released 1987). 

This program attempts to recover encryption keys for documents encrypted with Word Perfect 4.2. It has been tested to work with English based documents, written in Ascii. 

The tool came about because a couple of co-workers were discussing old files and one had an old word perfect document he wanted to try to decrypt. The other co-worker thought this would be a fun challenge, and this is the result of a few hours of research and coding.

Word Perfect's encryption algorithm in version 4.2 used an xor cipher. Since we didn't have any working binaries, the encryption/decryption algorithms were deciphered based on a paper written by Bennet, J[2] and a post by Helen Bergen. Using the information from these sources we were able to reconstruct the original algorithm. 

The algorithm is a simple xor symmetric algorithm. It uses a passphase as a key for ECB encryption mode as well as a second stream of sequenced bytes starting with keylength + 1 stored in a unsigned 8 bit integer, effectively: let sequence: u8 = keylength + 1. The sequence wraps as you would expect at 255 back to 0. 

This is effectively the same as breaking an XOR scheme since you can easily remove the sequence by "assuming" the keylength will be between n..m, adding m-n potential xor streams to crack. 

There are a few perf improvements that could be made. However, this can crack a 10 char password almost instantly on a modern pc. 

#Usage

wp_crack --encrypt-file [some_file] --key [a_key] 

This will take the original file, encrypt it and output it in the current directory with the .enc extension.

wp_crack --decrypt-file [some_encrypted_file] --key [a_key]

This will take the encrypted file and decrypt it, leaving an unencrypted copy in the directory as .dec. 

wp_crack --crack-file [some_encrypted_file] --key " " --depth 5 --min 3 --max 12

This will attempt to crack the file. The depth is used for the frequency matrix. Min/max is the size of the potential password. Longer the key, the longer it takes. The key in this instance will be the 'frequent' char. Try with space or e. 

##Resources

[1] H. A. Bergen and W. J. Caelli. 1991. File security in WordPerfect 5.0. Cryptologia 15, 1 (January 1991), 57-66. DOI=http://dx.doi.org/10.1080/0161-119191865795 (reproduced in a 1990 USENET on sci.crypt: https://groups.google.com/d/msg/sci.crypt/PmxaYcslekE/omwi6aIAPV4J)

[2] John Bennett. 1987. Analysis of the encryption algorithm used in the WordPerfect processing program. Cryptologia 11, 4 (October 1987), 206-210. DOI=http://dx.doi.org/10.1080/0161-118791862027


