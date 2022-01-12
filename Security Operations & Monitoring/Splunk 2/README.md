
# Splunk 2

This is my walkthrough for the [Splunk 2](https://tryhackme.com/room/splunk2gcd5) on tryhackme.com

In this exercise, you assume the persona of Alice Bluebird, the analyst who successfully assisted Wayne Enterprises and was recommended to Grace Hoppy at Frothly (a beer company) to assist them with their recent issues.

**What Kinds of Events Do We Have?**

The SPL (Splunk Search Processing Language) command metadata can be used to search for the same kind of information that is found in the Data Summary, with the bonus of being able to search within a specific index, if desired. All time-values are returned in EPOCH time, so to make the output user readable, the eval command should be used to provide more human-friendly formatting.

In this example, we will search the botsv2 index and return a listing of all the source types that can be found as well as a count of events and the first time and last time seen.

**Resources:**

http://docs.splunk.com/Documentation/Splunk/latest/SearchReference/Metadata
https://www.splunk.com/blog/2017/07/31/metadata-metalore.html

**Metadata command:**

`| metadata type=sourcetypes index=botsv2 | eval firstTime=strftime(firstTime,"%Y-%m-%d %H:%M:%S") | eval lastTime=strftime(lastTime,"%Y-%m-%d %H:%M:%S") | eval recentTime=strftime(recentTime,"%Y-%m-%d %H:%M:%S") | sort - totalCount`

**Note:** This information is from the **Advanced Hunting APTs with Splunk** app.


## 100 Series questions

The questions below are from the BOTSv2 dataset, questions 100-104. Some additional questions were added. 

In this task, we'll attempt to help guide you to each question's answer.

**Note:** The approach outlined in this task is not the only approach to tackle each question. 

Reading the questions below, the focus is on Amber Turing and her communication with a competitor.

### Question 1

The first objective is to find out what competitor website she visited. What is a good starting point?

When it comes to HTTP traffic, the source and destination IP addresses should be recorded in logs. You need Amber's IP address.

You can start with the following command, `index="botsv2" amber`, and see what events are returned. Look at the events on the first page. 

Amber's IP address is visible in the events related to PAN traffic, but it's not straightforward. 

To get her IP address, we can hone in on the PAN traffic source type specifically. 

Command: `index="botsv2" sourcetype="pan:traffic"`

![Src IP](./Screenshots/3-1.png?raw=true 'Source IP')

From here, you should have Amber's IP address. You can build a new search query using this information.

It would be best if you used the **HTTP stream** source type in your new search query. 

Using Amber's IP address, construct the following search query. 

Command: `index="botsv2" IPADDR sourcetype="stream:HTTP"`

You must substitute **IPADDR** with **Amber's IP address**.

After this query executes, there are many events to sift through for the answer. How else can you narrow this down?

Look at the additional fields.

Another field you can add to the search query to further shrink the returned events list is the site field.

Think about it; you're investigating what competitor website Amber visited.

Expand the search query only to return the site field. Additionally, you can remove duplicate entries and display the results nicely in a table.

**Command:** `index="botsv2" IPADDR sourcetype="stream:HTTP" | KEYWORD site | KEYWORD site`

You must substitute **KEYWORD** with the Splunk commands to remove the duplicate entries and display the output in a table format. 

**Note:** The first **KEYWORD** is to remove the duplicate entries, and the second is to display the output in a table format.

The results returned to show the URIs that Amber visited, but which website is the one that you're looking for?

To help answer these questions: Who does Amber work for, and what industry are they in? 

The competitor is in the same industry. The competitor website now should clearly be visible in the table output.

**Extra:** You can also use the industry as a search phrase to narrow down the results to a handful of events (1 result to be exact).

**Command:** `index="botsv2" IPADDR sourcetype="stream:HTTP" *INDUSTRY* | KEYWORD site | KEYWORD site`

**Note** Use asterisks to surrond the search term.

### Question 2-7

Amber found the executive contact information and sent him an email. Based on question 2, you know it's an image.

Since you now know the competitor website, you can construct a more specific search query isolating the results to focus on Amber's HTTP traffic to the competitor website. 

**Command:** `index=botsv2" IPADDR sourcetype="stream:HTTP" COMPETITOR_WEBSITE`

Replace **COMPETITOR_WEBSITE** with the actual URI of the competitor website. 

You can expand on the search query to output the specific field you want in a table format for an easy-to-read format, as we did for the previous objective. 

Based on the image, you have the CEO's last name but not his first name. Maybe you can get the name in the email communication.

You can now draw your attention to email traffic, SMTP, but you need Amber's email address. You should be able to get this on your own. :)

Once you have Amber's email address, you can build a search query to focus on her email address and the competitor's website to find events that show email communication between Amber and the competitor. 

**Command:** `index="botsv2" sourcetype="stream:smtp" AMBERS_EMAIL COMPETITOR_WEBSITE`

Replace **AMBERS_EMAIL** with her actual email address. 

With the returned results from the above search query, you can answer your own remaining questions. :)

### Answer the questions below


#### Amber Turing was hoping for Frothly to be acquired by a potential competitor which fell through, but visited their website to find contact information for their executive team. What is the website domain that she visited?

![Website](./Screenshots/3-1.png?raw=true 'Website')

#### Amber found the executive contact information and sent him an email. What image file displayed the executive's contact information? Answer example: /path/image.ext

![Image](./Screenshots/3-2.png?raw=true 'Image')

#### What is the CEO's name? Provide the first and last name.

Query `index="botsv2" sourcetype="stream:smtp" AMBERS_EMAIL COMPETITOR_DOMAIN`

You will then have to do some digging around in the raw text of the emails.

![CEO Name](./Screenshots/3-3.png?raw=true 'CEO Name')

#### What is the CEO's email address?

![CEO Email](./Screenshots/3-4.png?raw=true 'CEO Email')

#### After the initial contact with the CEO, Amber contacted another employee at this competitor. What is that employee's email address?

![Contact](./Screenshots/3-5.png?raw=true 'Contact')

#### What is the name of the file attachment that Amber sent ot a contact at the competitor?

![Filename](./Screenshots/3-6.png?raw=true 'Filename')

#### What is Amber's personal email address?

In the last email that Amber sent to the contact, we can see that the content_body is encoded in base64.

![base64](./Screenshots/3-7.png?raw=true 'base64')

I used https://www.base64decode.org/ to decode the content_body.


