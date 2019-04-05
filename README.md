## Cryptography 101 lunch

The subject of Crytography has always been the subject of much media speculation and public interest, and has suffered the degree of distortion guaranteed of such topics. To an uninformed reader, it may appear that any meaningful understanding of Cryptography requires a strong mathematical background and knowledge of computer software. While this is certainly true of those aspiring to work with such systems, some valuable insights and ideas are conveyed well regardless of subject training.

### What is the problem?

We begin by characterising the problem that motivates some well known Cryptographic protocols. Suppose that two individuals Alice and Bob wish to communicate with each other through some medium (letters, perhaps), on some important matters. The problem arises that this communication could be attacked a malicious party, who decides to: 

- spy on the communication, hence compromising the **confidentiality** of the conversation
- forge messages in the communication, hence compromising the **authenticity** of messages
- seize all letters from the mailbox and preventing their delivery, hence compromising the **availability** of a communication system

It is easy to see that parties with truly malicious intent could cause damage by interfering in private communications such as those between a client and a bank, for example. Cryptography focuses on the former two of these problems, there is not much meaning to exploring any communication system when messages are arbitrarily stolen. We now set about the task of exploring systems that satisfy these requirements. 

### Building a secure system



```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/ChengSashankh/topics-over-lunch/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
