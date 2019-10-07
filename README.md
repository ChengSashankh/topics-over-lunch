Topics over lunch is written with the motivation of bringing fascinating topics in Computer Science to a general audience.

# Table of Contents
1. [Encryption over lunch](#encryption-over-lunch)
2. [Assymetric Encryption](#creating-a-shared-secret)

## Encryption over lunch

The subject of Crytography has always been the subject of much media speculation and public interest, and has suffered the degree of distortion guaranteed of such topics. To an uninformed reader, it may appear that any meaningful understanding of Cryptography requires a strong mathematical background. While this is certainly true of those aspiring to work with such systems, some valuable insights and ideas are conveyed well regardless of subject training.

### What is the problem?

We begin by characterising the problem that motivates some well known Cryptographic protocols. Suppose that two individuals Alice and Bob wish to communicate with each other through some medium (letters, perhaps), on some important matters. The problem arises that this communication could be attacked a malicious party, who decides to: 

- spy on the communication, hence compromising the **confidentiality** of the conversation
- forge messages in the communication, hence compromising the **authenticity** of messages
- seize all letters from the mailbox and preventing their delivery, hence compromising the **availability** of a communication system

It is easy to see that parties with truly malicious intent could cause damage by interfering in private communications such as those between a client and a bank, for example. Cryptography focuses on the former two of these problems; there is not much meaning to exploring any communication system when messages are arbitrarily stolen. 

### Protecting message privacy

We now set about the task of exploring systems that satisfy these requirements, step by step. As a first step we attempt to find a way to protect the confidentiality over the message. Suppose that Alice wishes to share with Bob a message `m = "Hello Bob"`. Sharing this message in its current form, would of course allow any eavesdropper gain the complete meaning of the message. 

However, suppose that we applied some transformation (called encryption) upon this message m, which resulted in some other seemingly unrelated string,(perhaps `and1c3de5jd` or any other string you can write down) and only shared this over the communication system. Unless the eavesdropper had knowledge of the exact way in which this transformation was applied, he/she has no way of recovering the message m, and hence is not aware that the message exchanged was indeed `Hello Bob`. If Bob knows a method to transform the secret message back to its original form (called decryption), then in a very primitive sense, this system allows Alice and Bob to communicate without anyone eavesdropping on the conversation. Anyone may acquire the encrypted string, but only Alice and Bob are able to derive the actual meaning from them.

In order for this to be useful, we require that:
- Alice and Bob agree on an encryption and decryption method.
- no one else knows of these methods.

In real life, it is neither possible nor desirable that every pair of communicating parties agree on a new and secret method of communication. Instead we consider an alternate approach where Alice and Bob agree on a secret password (called a secret key) before hand. We modify the above system such that:

- one can only decrypt a message if the key with which it was encrypted is known.
- all communicating parties use the same encryption and decryption method.

This is analogous in some sense to the way we generally log into our email accounts. That a username and password are to be typed in are common knowledge, however only your specific password unlocks your account. In other words, the **protocol**, or process to unlock an account is publicly known and common to everyone, yet impossible without your secret key. 

So Alice and Bob now agree on a secret key beforehand, and choose a publicly known encryption and decryption method. When Alice sends `Hello Bob`, she encrypts it with the function and that specific secret key. Bob then uses the same secret key to decrypt the message. The eavesdropper obtains the encrypted message, and knows the method to decrypt a message in general, but is unable to do so without the secret key. 

![Figure 1: Encryption and Decryption](images/Figure_1.png)

This is one simple way Alice and Bob can encrypt their messages to keep them secret from eavesdroppers. This systems seems to work, provided that:

- Alice and Bob have agreed on a secret key before hand, and nobody else knows of this key.
- we know of suitable encryption and decryption functions.

The system suggested above certainly still is not without issues. If a third party guesses the key correctly, then he/she will be able to read all the messages. It must hence be hard to guess the key. Alice and Bob will also need some way to secretly agree on such a key. Without this, this method will not work at all. Additionally, it provides Bob with no way to verify that Alice indeed sent the messages, and not someone else. 

The system discussed above is called **symmetric encryption**, since encryption and decryption both _symmetrically_ use the same secret key. There are other methods where different keys are used.

In future articles, we will discuss other methods that improve on the security of this scheme.

## Creating a Shared Secret

In the previous section, we discussed the benefits of encrypting communications for the benefit of privacy. While this might seem sufficiently secure at a first glance, we will see that the practicalities of the real world demand the specification of a number of other schemes and processes in order to enable this encryption. 

For example, the scheme discussed above (referred to as **symmetric encryption**) require Alice and Bob to share a secret key that no other party is aware of. It is unclear so far as to how such a secret may be generated in the event that the parties cannot agree beforehand on such schemes through some other means. In this section, we discuss this problem further, and open a broader discussion on the practicalities of symmetric encryption.

Let us begin again by characterizing the problem that motivates this discussion. Suppose that Alice and Bob are in different parts of the world, and wish to communicate without anyone else eavesdropping on their communications. Recall from the previous section that they can use symmetric encryption to do so, provided they somehow agree upon a secret key without others realizing this key. This requires that they first negotiate a secret key, before communicating. The problem now distills to the following:

```
Given that a third party can listen in on all possible communications between Alice and Bob, how can they agree upon a common secret that others do not know about?
```

In the context of the internet, it is possible that Alice and Bob have never met before (Alice could be a server in different country). It is hence not possible, or practical to rely on shared secret information to create a key. We must create a secret in plainsight.

The following is a brief description of the approach. Please note that this section compromises some mathematical accuracies in the interest of simplicity.

In 1976, researchers proposed the Diffie-Hellman protocol to address this problem. The approach to solving this problem begins in a seemingly disconnected mathematical property of numbers: `that under some conditions, it is easy to calculate the exponents of numbers, but  much harder to reverse this process`. This means that in certain groups, 

- calculating g<sup>x</sup> given `g` and `x` is a simple problem, and,
- calculating `x` given `g` and g<sup>x</sup> is a much harder problem

(ignore the semantics of the mathematical structure `Group` if it is not relevant to you) 

It can now be seen how this mathematical property can be used to create a shared secret. Observe the following interaction. In this discussion, Alice and Bob are attempting to negotiate a secret key and Eve is attempting to listen in on the conversation and steal the key:

- Alice randomly selects a number `x` and calculates g<sup>x</sup>. As we discussed before, this can be achieved easily since it is efficiently calculable. She tells Bob, "Hi, I'd like to chat and here are two numbers g<sup>x</sup> and g."
- Eve can hear this, and she writes down both g and g<sup>x</sup>
- Bob notes down the same details. He now samples a random number y, and similarly calculates g<sup>y</sup>. He tells Alice, "Hi, here are my two numbers g and g<sup></sup>"
- Eve notes these down as well
- Alice receives this and notes down as well. 

At this point, each party has the following details:

- Alice: x, g, g<sup>x</sup> and g<sup>y</sup>
- Bob: y, g, g<sup>y</sup> and g<sup>x</sup>
- Eve: g, g<sup>x</sup> and g<sup>y</sup>

Suppose they each attempt to calculate g<sup>xy</sup>:
- Alice can raise g<sup>y</sup> to the power x and obtain g<sup>xy</sup>
- Bob can raise g<sup>x</sup> to the power y and obtain g<sup>yx</sup>, which is the same as g<sup>xy</sup>
- Even cannot calculate g<sup>xy</sup> directly, without first calculating either x or y. As we discussed earlier, the problem of obtaining x or y from g<sup>x</sup> or g<sup>y</sup> respectively is considered very hard if the suitable g is chosen. 

Hence, Alice and Bob can generate a shared secret key in plain sight. Remember, these properties are not true of all numbers. They are exclusively true of some mathematical groups, and a further discussion on this would be non trivial. 

The above protocol allows use to now see completely how two unrelated parties can communicate a shared secret key in the presence of malicious actors. They first employ this key-exchange protocol to obtain the shared secret key g<sup>xy</sup> and then proceed to use that to symmetrically encrypt all communications. 

The practicality of symmetric encryption is now vastly extended. It can be possible for two parties who have never met before, to communicate confidentially without having to share any secrets before hand. This brings the simplicity of symmetric encryption to use in the era of the internet. 

Even with this protocol, a multitude of issues remain unresolved and undiscussed. We will further our discussion of secure communication in later sections.