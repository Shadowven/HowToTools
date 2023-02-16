# Yara

## 1. Introduction

Yara (Yet Another Ridiculous Acronym) is and its importance in infosec today. Yara was developed by Victor M. Alvarez ([@plusvic](https://twitter.com/plusvic)) and (@VirusTotal)[https://twitter.com/virustotal]. Check the GitHub repo (here)[https://github.com/virustotal/yara].

## 2. What is Yara?

### **All about Yara**

*"The pattern matching swiss knife for malware researchers (and everyone else)" (Virustotal., 2020)*

Yara can identify information based on both binary and textual patterns, such as hexadecimal and strings contained within a file.

Rules are used to label these patterns. For example, Yara rules are frequently written to determine if a file is malicious or not, based upon the features - or patterns - it presents. Strings are a fundamental component of programming languages. Applications use strings to store data such as text.

For example, the code snippet below prints "Hello World" in Python. The text "Hello World" would be stored as a string.

`print("Hello World!")`

We could write a Yara rule to search for "hello world" in every program on our operating system if we would like. 