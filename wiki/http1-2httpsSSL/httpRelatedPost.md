---
layout: page
title:  HTTP vs HTTP 2 vs HTTPs with SSL
permalink: /wiki/http1-2httpsSSL/
---

*by:* Weizhao Li & Ziyue Li & Zhuolun Gao


A short description of your post goes here.

---

The rest of your post goes here.

Hypertext Transfer Protocol Secure (HTTPS) is an extension of HTTP. It can also be called with HTTP over SSL (or TLS), which indicates its high relation with SSL protocol. 
Now,let’s take a brieft journey on how HTTPS evolved and take a closer look at the basic concepts of the it.

The defect of HTTP
There are three main defects that HTTP hold:
•	Communication uses clear text (not encrypted), which may be eavesdropped.
•	The identity of the correspondent is not verified, so there may be spoofing.
•	The integrity of the message cannot be proven, so it may have been tampered with.
To solve these questions and get a more safe communication, SSL (or TLS) are introduced.

Brief history of SSL
Secure Sockets Layer (SSL), and its now standardized successor, Transport Layer Security (TLS), was first developed by Netscape Company. Taher Elgamal(Figure 1), chief scientist at Netscape Communications from 1995 to 1998, has been described as the "father of SSL". SSL Version 1.0 was never publicly released because of serious security flaws in the protocol. Version 2.0, released in February 1995, contained a number of security flaws which necessitated the design of Version 3.0.
 
Figure 1. Taher Elgamal
TLS 1.0 was first defined in RFC 2246 in January 1999 as an upgrade of SSL Version 3.0, and written by Christopher Allen and Tim Dierks of Consensus Development. As stated in the RFC, "the differences between this protocol and SSL 3.0 are not dramatic, but they are significant enough to preclude interoperability between TLS 1.0 and SSL 3.0". TLS 1.0 does include a means by which a TLS implementation can downgrade the connection to SSL 3.0, thus weakening security.
After that, several version of TLS developed to make improvement on security and architecture. Here is a list of different vesion of SSL and TLS.






Form 1. SSL and TLS protocols
Protocol	Published	Status
SSL 1.0	Unpublished	Unpublished
SSL 2.0	1995	Deprecated in 2011 (RFC 6176)

SSL 3.0	1996	Deprecated in 2015 (RFC 7568)

TLS 1.0	1999	Deprecation planned in 2020
TLS 1.1	2006	Deprecation planned in 2020
TLS 1.2	2008	
TLS 1.3	2018	

Therefore, HTTPS can be seen a combination work between HTTP and SSL/TLS, as Form 2 illustated.
Form 2. The difference between HTTP and HTTPS on network layer architecture
HTTP	HTTPS
Application	HTTP	Application	HTTP
		Security	TLS/SSL
Transport	TCP	Transport	TCP
Network	IP	Network	IP
Datalink	MAC	Datalink	MAC


Basic cryptography
There are two cryptography to be showed here: Symmetric-key algorithm and asymmetric cryptography. Both cryptography approaches will be used to secure communications while guarantee the performance.


Cleartext
 
Figure 2. Cleartext transmission is easy to be eavesdropped

Symmetric-key encryption
 
Figure 3. Illustration of Symmetric-key encrypted transmission
Symmetric-key algorithms are algorithms for cryptography that use the same cryptographic keys for both encryption of plaintext and decryption of ciphertext. The keys may be identical or there may be a simple transformation to go between the two keys. The keys, in practice, represent a shared secret between two or more parties that can be used to maintain a private information link. This requirement that both parties have access to the secret key is one of the main drawbacks of symmetric key encryption(shows on Figure 4), in comparison to public-key encryption (also known as asymmetric key encryption)
 
Figure 4. How to transfer key itself safely?



Asymmetric key encryption
Public-key cryptography, or asymmetric cryptography, is a cryptographic system that uses pairs of keys: public keys which may be disseminated widely, and private keys which are known only to the owner. The generation of such keys depends on cryptographic algorithms based on mathematical problems to produce one-way functions. A one-way function is a function that is easy to compute on every input, but hard to invert given the image of a random input in the sense of computational complexity theory. Effective security only requires keeping the private key private; the public key can be openly distributed without compromising security.
As Figure 5 shows, an unpredictable (typically large and random) number is used to begin generation of an acceptable pair of keys suitable for use by an asymmetric key algorithm.
 
Figure 5. Illustration on key pairs generation process.

 
Figure 6. Illustration of Asymmetric key encrypted communication


Figure 6 shows the basic step of using key pairs to communicate safely. At first, the client will request server for its public keys, and the server send its public key back to the client. Although this step both request and public key itself is in cleartext, this step will guarantee the next step’s security. 
Middleman Attack
Although asymmetric key encryption seemed to be a way to communicate safely, problems still exist—the middleman attack.
a man-in-the-middle attack (MITM) is an attack where the attacker secretly relays and possibly alters the communications between two parties who believe that they are directly communicating with each other. One example of a MITM attack is active eavesdropping, in which the attacker makes independent connections with the victims and relays messages between them to make them believe they are talking directly to each other over a private connection(as Figure 7 shows), when in fact the entire conversation is controlled by the attacker. The attacker must be able to intercept all relevant messages passing between the two victims and inject new ones. This is straightforward in many circumstances; for example, an attacker within reception range of an unencrypted wireless access point (Wi-Fi) could insert themselves as a man-in-the-middle.
 

Figure 7. Illustration of Middleman attack
 
Solution: Certificate Authority & Digital Certificates
Now it is time to introduce the almost-perfect solution: Certificate Authority and Digital Certificate. A certificate authority or certification authority (CA) is an entity that issues digital certificates. The issuing process is showed on Figure 8. A digital certificate certifies the ownership of a public key by the named subject of the certificate. This allows others (relying parties) to rely upon signatures or on assertions made about the private key that corresponds to the certified public key. A CA acts as a trusted third party—trusted both by the subject (owner) of the certificate and by the party relying upon the certificate.
 
Figure 8. Digital Certificate Issuing Process
As showed on Figure 9, the Digital Certificate will used to make the first step identification and exchange the random key, which will be used later in a format of symmetric-key encryption in order to improve the performance.

 
Figure 9. Communication with Digital Certificate between client and server












Because of the encryption process, HTTPS will incur higher latency than HTTP/1. Here is a small experience to show that.

First, there are some python codes. I recommend you to run the code on jupyter notebooks.
import requests
import time

i = 0
url="http://zli.name"
l =list()
while i < 100:
    r = requests.get(url)
    time.sleep(1)
    print (r.elapsed.total_seconds())
    l.append(r.elapsed.total_seconds())
    i = i + 1
print(l)
Above, the code will request zli.name, which is my newly build personal blow website based on wordpress for 100 times and restore the time elapsed in each request in a list.
Then, I will use bellowed code to draw a histogram to show the time needed for zli.name to response.
data = [1,2,3]
%matplotlib inline
import matplotlib.pyplot as plt
 
plt.xlabel("L")
plt.ylabel("#")
plt.title("http://zli.name")
plt.hist(l)

The result is showed here.
 

While, I will do the nearly samething for the https://zli.name, everything is same with above, just change the value of url from http://zli.name to https://zli.name.

The result shows below.
 
You can see that, while indicated by the red circle, the HTTPS is relatively causing more delay than HTTP.
Below shows the protocol is HTTP/1. By the developer function provided by Chrome.
 

I have an idle domain name and last night I build a website based on wordpress with it. I think this process can give you a direct show of how to install SSL certificate and thus made the website you created more safely.
At first, my website shows something like this.
 

 

 
Then, I use Let’s encrypt to install certificate on my browser. When you choose your OS and your server end, Certbot will show customized instructions.
This is what I choose:
 
Instrcutions are below:
First, ssh into ther server of your website.
Second, you'll need to add the Certbot PPA to your list of repositories. To do so, run the following commands on the command line on the machine:
 
Third, Install Certbot. Run this command on the command line on the machine to install Certbot.
 
Last, Run this command to get a certificate and have Certbot edit your Nginx configuration automatically to serve it, turning on HTTPS access in a single step.
 
After that, you can see that the website can be recognized as safe through Chrome browser.

 


References
https://en.wikipedia.org/wiki/Certificate_authority
https://en.wikipedia.org/wiki/Man-in-the-middle_attack
https://en.wikipedia.org/wiki/One-way_function
https://en.wikipedia.org/wiki/Ciphertext
https://en.wikipedia.org/wiki/Taher_Elgamal
https://en.wikipedia.org/wiki/Transport_Layer_Security
https://en.wikipedia.org/wiki/Symmetric-key_algorithm
https://en.wikipedia.org/wiki/Public-key_cryptography
https://en.wikipedia.org/wiki/HTTPS
https://blog.csdn.net/freekiteyu/article/details/76423436
https://certbot.eff.org/
https://letsencrypt.org/

