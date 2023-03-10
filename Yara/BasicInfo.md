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

### **Why does Malware use Strings?**

Malware, just like our "Hello World" application, uses strings to store textual data. Here are a few examples of the data that various malware types store within strings:

| Type | Data | Description |
| :---        |    :----:   |          ---: |
| Ransomware | [12t9YDPgwueZ9NyMgw519p7AA8isjr6SMw](https://www.blockchain.com/btc/address/12t9YDPgwueZ9NyMgw519p7AA8isjr6SMw) | Bitcoin Wallet for ransom payments |
| Botnet | 12.34.56.7 | The IP address of the Command and Control (C&C) server |

## 3. Download and Installation

-Linux

`apt-get install yara apt-get install python-yara /pip install yara `

-Windows

```https://www.dropbox.com/sh/umip8ndplytwzj1/AADdLRsrpJL1CM1vPVAxc5JZa?dl=0&lst=```

```pip install yara-python```

## 4. Introduction to Yara Rules

### Your First Yara Rule

The proprietary language that Yara uses for rules is fairly trivial to pick up, but hard to master. This is because your rule is only as effective as your understanding of the patterns you want to search for.

Using a Yara rule is simple. Every `yara` command requires two arguments to be valid, these are:
1) The rule file we create
2) Name of file, directory, or process ID to use the rule for.

Every rule must have a name and condition.

For example, if we wanted to use "myrule.yar" on directory "some directory", we would use the following command:
`yara myrule.yar somedirectory`

Note that **.yar** is the standard file extension for all Yara rules. We'll make one of the most basic rules you can make below.

1. Make a file named "**somefile**" via `touch somefile`
2. Create a new file and name it "**myfirstrule.yar**" like below:

    ![firstfile](firstfile.png)

3. Open the "myfirstrule.yar" using a text editor such as `nano` and input the snippet below and save the file:

    ```
    rule examplerule {
            condition: true
    }
    ```
    ![nano](nano.png)

The name of the rule in this snippet is `examplerule`, where we have one condition - in this case, the **condition** is `condition`. As previously discussed, every rule requires both a name and a condition to be valid. This rule has satisfied those two requirements.

Simply, the rule we have made checks to see if the file/directory/PID that we specify exists via `condition: true`. If the file does exist, we are given the output of `examplerule`

Let's give this a try on the file "somefile" that we made in step one:
`yara myfirstrule.yar somefile`

If "somefile" exists, Yara will say `examplerule` because the pattern has been met - as we can see below:

If "somefile" exists, Yara will say examplerule because the pattern has been met - as we can see below and If "somefile" exists, Yara will say examplerule because the pattern has been met - as we can see below:

![file](file.png)

## 5.  Expanding on Yara Rules 

### **Yara Conditions Continued...**

Checking whether or not a file exists isn't all that helpful. After all, we can figure that out for ourselves...Using much better tools for the job.

Yara has a few conditions, which I encourage you to read here at your own leisure. However, I'll detail a few below and explain their purpose.

| Keyword |
| ----------- |
| Desc |
| Meta |
| Strings |
| Conditions |
| Weight |

### **Meta**
This section of a Yara rule is reserved for descriptive information by the author of the rule. For example, you can use desc, short for description, to summarise what your rule checks for. Anything within this section does not influence the rule itself. Similar to commenting code, it is useful to summarise your rule.


### **Strings**

Remember our discussion about strings in Task 2? Well, here we go. You can use strings to search for specific text or hexadecimal in files or programs. For example, say we wanted to search a directory for all files containing "Hello World!", we would create a rule such as below:

```
rule helloworld_checker{
	strings:
		$hello_world = "Hello World!"
}
```

We define the keyword `Strings` where the string that we want to search, i.e., "Hello World!" is stored within the variable `$hello_world`

Of course, we need a condition here to make the rule valid. In this example, to make this string the condition, we need to use the variable's name. In this case, `$hello_world`:

```
rule helloworld_checker{
	strings:
		$hello_world = "Hello World!"

	condition:
		$hello_world
}
```
Essentially, if any file has the string "Hello World!" then the rule will match. However, this is literally saying that it will only match if "Hello World!" is found and will not match if "*hello world*" or "*HELLO WORLD*."

To solve this, the condition `any of them` allows multiple strings to be searched for, like below:

```
rule helloworld_checker{
	strings:
		$hello_world = "Hello World!"
		$hello_world_lowercase = "hello world"
		$hello_world_uppercase = "HELLO WORLD"

	condition:
		any of them
}
```

Now, any file with the strings of:
1. Hello World!
2. hello world
3. HELLO WORLD

Will now trigger the rule.

### **Conditions**

We have already used the true and any of them condition. Much like regular programming, you can use operators such as:

        <= less than or equal to
        >= more than or equal to
        != not equal to

For example, the rule below would do the following:

```
rule helloworld_checker{
	strings:
		$hello_world = "Hello World!"

	condition:
        #hello_world <= 10
}
```
The rule will now:

1. Look for the "Hello World!" string
2. Only say the rule matches if there are less than or equal to ten occurrences of the "Hello World!" string

### **Combining keywords**

Moreover, you can use keywords such as:

        and
        not
        or 

To combine multiple conditions. Say if you wanted to check if a file has a string and is of a certain size (in this example, the sample file we are checking is __less than__ <10 kb and has "Hello World!" you can use a rule like below:

```
rule helloworld_checker{
	strings:
		$hello_world = "Hello World!" 
        
        condition:
	        $hello_world and filesize < 10KB 
}
```

## 6. Yara Modules

### **Integrating With Other Libraries**

Frameworks such as the [Cuckoo Sandbox](https://cuckoosandbox.org/) or [Python's PE Module](https://pypi.org/project/pefile/) allow you to improve the technicality of your Yara rules ten-fold.


### **Cuckoo**

Cuckoo Sandbox is an automated malware analysis environment. This module allows you to generate Yara rules based upon the behaviours discovered from Cuckoo Sandbox. As this environment executes malware, you can create rules on specific behaviours such as runtime strings and the like.


### **Python PE**
Python's PE module allows you to create Yara rules from the various sections and elements of the Windows Portable Executable (PE) structure.


Examining a PE file's contents is an essential technique in malware analysis; this is because behaviours such as cryptography or worming can be largely identified without reverse engineering or execution of the sample.

## 7. Other tools and Yara

### Yara Tools

Knowing how to create custom Yara rules is useful, but luckily you don't have to create many rules from scratch to begin using Yara to search for evil. There are plenty of GitHub [resources](https://github.com/InQuest/awesome-yara) and open-source tools (along with commercial products) that can be utilized to leverage Yara in hunt operations and/or incident response engagements. 


### LOKI

LOKI is a free open-source IOC (Indicator of Compromise) scanner created/written by Florian Roth.

Based on the GitHub page, detection is based on 4 methods:

1. File Name IOC Check
2. Yara Rule Check __(we are here)__
3. Hash Check
4. C2 Back Connect Check

There are additional checks that LOKI can be used for. For a full rundown, please reference the [GitHub readme](https://github.com/Neo23x0/Loki/blob/master/README.md).

LOKI can be used on both Windows and Linux systems and can be downloaded [here](https://github.com/Neo23x0/Loki/releases).

![loki](loki.png)

### THOR

THOR Lite is Florian's newest multi-platform IOC AND YARA scanner. There are precompiled versions for Windows, Linux, and macOS. A nice feature with THOR Lite is its scan throttling to limit exhausting CPU resources. For more information and/or to download the binary, start [here](https://www.nextron-systems.com/thor-lite/). You need to subscribe to their mailing list to obtain a copy of the binary. **Note that THOR is geared towards corporate customers**. THOR Lite is the free version.

### FENRIR

This is the 3rd [tool](https://github.com/Neo23x0/Fenrir) created by Neo23x0 (Florian Roth). You guessed it; the previous 2 are named above. The updated version was created to address the issue from its predecessors, where requirements must be met for them to function. Fenrir is a bash script; it will run on any system capable of running bash (nowadays even Windows). 

### YAYA(Yet Another Yara Automaton)

YAYA was created by the [EFF](https://www.eff.org/deeplinks/2020/09/introducing-yaya-new-threat-hunting-tool-eff-threat-lab) (*Electronic Frontier Foundation*) and released in September 2020. Based on their website, *"YAYA is a new open-source tool to help researchers manage multiple YARA rule repositories. YAYA starts by importing a set of high-quality YARA rules and then lets researchers add their own rules, disable specific rulesets, and run scans of files."*

**Note**: Currently, YAYA will only run on <u>Linux</u> systems.

## 8. Using LOKI

As a security analyst, you may need to research various threat intelligence reports, blog postings, etc. and gather information on the latest tactics and techniques used in the wild, past or present. Typically in these readings, IOCs (hashes, IP addresses, domain names, etc.) will be shared so rules can be created to detect these threats in your environment, along with Yara rules. On the flip side, you might find yourself in a situation where you've encountered something unknown, that your security stack of tools can't/didn't detect. Using tools such as Loki, you will need to add your own rules based on your threat intelligence gathers or findings from an incident response engagement (forensics). 

As mentioned before, Loki already has a set of Yara rules that we can benefit from and start scanning for evil on the endpoint straightaway.

**Note**:

   1. Run `python loki.py -h` to see what options are available.

   2. If you are running Loki on your own system fiest time, the first command you should run is `--update`. This will add the `signature-base` directory, which Loki uses to scan for known evil.

To run Loki, you can use the following command
(note that I am calling Loki from within the file 1 directory which is the malwares I've prepared for testing ):

![run](run.png)

We can check the sample reasults:

![warning](warning.png)

From above we can see:
1. Does Loki detect this file as **suspicious**
2. Yara rule match on **webshell_metaslsoft**
3. Loki classify this file as **Web Shell**
4. **b374k 2.2** is the name and version of this hack tool

## 9. yarGen

### Creating Yara rules with yarGen

Sometimes we also need to create a Yara rule to detect this specific web shell in our environment, from below we can see the Loki didn't flag on file 2 and it will go undetected.

![fail](fail.png)

We can manually open the file and attempt to sift through lines upon lines of code to find possible strings that can be used in our newly created Yara rule.

Foring checking how many lines this particular file has you can run the following: ```strings <file name> | wc -l```.

![huge](huge.png)

If you try to go through each string, line by line manually, you should quickly realize that this can be a daunting task. 

Luckily, we can use [yarGen](https://github.com/Neo23x0/yarGen) (yes, another tool created by Florian Roth) to aid us with this task.

What is yarGen? yarGen is a generator for YARA rules.

From the README - "*The main principle is the creation of yara rules from strings found in malware files while removing all strings that also appear in goodware files. Therefore yarGen includes a big goodware strings and opcode database as ZIP archives that have to be extracted before the first use.*"

**Note**: If you are running yarGen on your own system first time, you need to update it first by running the following command: `python3 yarGen.py --update`.

![yarGen](yarGen.png)

This will update the good-opcodes and good-strings DB's from the online repository. This update will take a few minutes. 

 Once it has been updated successfully, you'll see the following message at the end of the output.




To use yarGen to generate a Yara rule for file 2, you can run the following command:

```python3 yarGen.py -m /home/cmnatic/suspicious-files/file2 --excludegood -o /home/cmnatic/suspicious-files/file2.yar ```

A brief explanation of the parameters above:
- `-m` is the path to the files you want to generate rules for
- `--excludegood` force to exclude all goodware strings (these are strings found in legitimate software and can increase false positives)
- `-o` location & name you want to output the Yara rule

If all is well, you should see the following output.

![py](py.png)

1. From within the root of the suspicious files directory, running command below to test Yara and your Yara rule against file 2: 
   
   ```yara file2.yar file2/1ndex.php```

2. Yara rule flag file 2
   
   ![flag](flag.png)

3. Copy the Yara rule  into the Loki signatures directory:
   
   ```cp file2.yar ~/tools/Loki/signature-base/yara```

   ![cp](cp.png)

4. Test the Yara rule with Loki, and run the command below:

	```python ~/tools/Loki/loki.py -p ~/suspicious-files/file2```

	![result](result.png)


**Note**: Another tool created to assist with this is called [yarAnalyzer](https://github.com/Neo23x0/yarAnalyzer/) (created by Florian Roth). We will not examine that tool in this room, but you should read up on it, especially if you decide to start creating your own Yara rules. 

**Further Reading on creating Yara rules and using yarGen:**

- https://www.bsk-consulting.de/2015/02/16/write-simple-sound-yara-rules/
- https://www.bsk-consulting.de/2015/10/17/how-to-write-simple-but-sound-yara-rules-part-2/
- https://www.bsk-consulting.de/2016/04/15/how-to-write-simple-but-sound-yara-rules-part-3/


## 10. Valhalla