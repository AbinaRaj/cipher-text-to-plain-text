# cipher-text-to-plain-text

import sys
import os
import binascii
from Crypto.Util import RFC1751

from Crypto.Cipher import AES
import hashlib
import sys
import binascii
import Padding

plaintext='hello'
password='hello'

if (len(sys.argv)>1):
	plaintext=str(sys.argv[1])

plaintext=plaintext.replace("#"," ")


def encrypt(plaintext,key, mode):
	encobj = AES.new(key,mode)
	return(encobj.encrypt(plaintext))

def decrypt(ciphertext,key, mode):
	encobj = AES.new(key,mode)
	return(encobj.decrypt(ciphertext))


key = hashlib.sha256(password.encode()).digest()


plaintext = Padding.appendPadding(plaintext,blocksize=Padding.AES_blocksize,mode=0)
print("Input data (CMS): ",binascii.hexlify(plaintext.encode()))
print()
ciphertext = encrypt(plaintext.encode(),key,AES.MODE_ECB)
print("Cipher (ECB): ",binascii.hexlify(bytearray(ciphertext)))

print()

y = RFC1751.key_to_english(ciphertext)
print("Cipher to plain:\t",y)


ciphertext = RFC1751.english_to_key(y)
print("\n\nReverse:\t",binascii.hexlify(key))

print()
plaintext = decrypt(ciphertext,key,AES.MODE_ECB)
print("  decrypt: ",plaintext)
plaintext = Padding.removePadding(plaintext.decode(),mode=0)
print("  decrypt: ",plaintext)
