---
layout: post
title: "WhatsBook"
description: "Paper archive of your whatsApp conversations"
date: 2017-4-13
tags: [python, image processing, LaTeX]
comments: True
HN: 14107312
share: true
---

The idea of archiving of your WhatsApp conversations in printed form seemed beautiful. I first came across this idea in a blog post by [Pelle Beckman](https://medium.com/@pbeck/whatsapp-books-a-hacker-s-guide-edbb397e0bee). The project below is my take on this beautiful idea. 

![WhatsBook a photo book styled chat archive](/assets/whatsBook/photoBook.jpg "WhatsBook a photo book styled chat archive")

WhatsApp has a little know feature that allows one to export a certain chat to a parsable format. After extracting the resulting export has a single `_chat.txt` file that contains the entire chat conversation. Additionally the different attachments are present with a simple time stamp and numerical index. 

```
28-09-16 11:20:19: Tara: Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vivamus vel mattis mi, in posuere tortor. Sed sed ipsum bibendum, facilisis enim in, laoreet justo. 
28-09-16 11:20:33: Alex: Aenean lacinia dapibus pharetra. Phasellus venenatis congue dolor, ut commodo lorem porta id. Quisque iaculis velit sed velit dignissim malesuada. 
28-09-16 17:00:04: Tara: 3.jpg <attached>
28-09-16 12:10:51: Filip: Integer quis arcu vitae tellus sollicitudin lobortis non vitae dolor. 
28-09-16 12:13:42: Alex: In imperdiet tortor ipsum, vel aliquet erat condimentum quis. Interdum et malesuada fames ac ante ipsum primis in faucibus. Phasellus pharetra ultricies efficitur.
```

Inspired by the work of Pelle, I have built a small python script to parse the exported chat data (that looks like the snippet above) to allow for rendering using LaTeX (in my case pdfLaTeX). The result is a tiny A5 sized book containing all of your conversations and images, as seen in the images below. All the code can be found on the [GitHub repository](https://github.com/theveloped/WhatsBook).

<a href="/assets/whatsBook/whatsBook.pdf">
<img style="float:left; margin-right: 6px; box-shadow: 2px 2px 5px 0px rgba(0,0,0,0.5);" src="/assets/whatsBook/september.jpg">
<img style="float:left; margin-right: 6px; box-shadow: 2px 2px 5px 0px rgba(0,0,0,0.5);" src="/assets/whatsBook/page1.jpg">
<img style="float:left; box-shadow: 2px 2px 5px 0px rgba(0,0,0,0.5);" src="/assets/whatsBook/page2.jpg">
</a>

## Line by line

The main process is fairly straightforward: simply go over the lines one by one and change the formatting to match the desired LaTeX template/syntax. 

```
28-09-16 11:20:19: Tara: Lorem ipsum dolor sit amet 
```

A standard line is split into its time-stamp, sender and content using simple regular expressions. And is then parsed into to following format:

```
\textbf{Tara}: Lorem ipsum dolor sit amet \\
```

Possible emoji characters are covered by a solution covered [here](https://github.com/alecjacobson/coloremoji.sty). Apart from some edge cases as chats containing line breaks or reserved LaTeX characters, no further magic is applied. If a line indicates an attachment further processing is done to include the respective image (only still images are supported for now).


## Images included

Image attachments are first checked for availability and if so included in pairs. The layout here is not chronologically but governed by aspect ratio. The script tries to add the image pair in such a sequence that when scaled to the same hight the divider margin alternates between the left and right half of the page. In case of many images this results in a layout that is visually similar to a running bond in brickwork.

![Images spaced with divider on the left](/assets/whatsBook/leftSpace.jpg "Images spaced with divider on the left")
![Images spaced with divider on the right](/assets/whatsBook/rightSpace.jpg "Images spaced with divider on the right")

The images above show the layout of the images with the divider placed either in the right or on the left. If the section (day) ends, any remaining images will be rendered as a single full width image.


## Date parsing

The main layout of the book is chronological, dividing the chat into chapters by month and sections by day. For this the time-stamp is first parsed into a text based representation of the date and month.

```python
# parse date string of the following formant: 29/12/15 
def parseDate(dateString):

  day, month, year = re.split("[-/]", dateString, 2)
  date = datetime.date(int("20" + year), int(month), int(day))

  if 4 <= int(day) <= 20 or 24 <= int(day) <= 30:
      suffix = "th"
  else:
      suffix = ["st", "nd", "rd"][int(day) % 10 - 1]

  parsedDate = date.strftime('%A') + " $" + day + "\\textsuperscript{" + suffix + "}$"
  return (parsedDate, date.strftime("%B"))
```

The small snippet above converts the time-stamp as found in the exported chat to a spelled out representation of both the date and month.


## Chapters a.k.a. months

As described above the chapters indicate months. For this a new chapter line is added to the output on reading a timestamp belonging to a new month. The full page chapter image is generated on completion of the month using Andreas Mueller [word cloud generator](http://amueller.github.io/word_cloud/) and the preceding chat content.

![Word cloud chapter image](/assets/whatsBook/wordcloud.jpg "Word cloud chapter image")

The resulting chapter image gives an instant overview of some of the conversation topics of that month and allows for a rather simple automation of custom chapter images. Note the generation of the word cloud images is by far the most expensive operation in the script. For this reason I've added it as an optional flag one would most likely only use on generating the final version.


## Sections a.k.a. days

The sections are implemented in a similar way as introduced by Pelle. I found the single line section headers by day to be a good trade off between readability and space. The sections give enough insight in the time scale of the conversation while leaving most of the page free for the real content.

```
\section*{Wednesday $28\textsuperscript{th}$}
```

As previously discussed the date is parsed to a spelled out form of the date that now allows for a simple inclusion of the section as seen above.


## Printing

The only thing remaining is to render the parsed chat to a pdf using LaTeX. The current LaTeX page setup fits the A5 portrait specs at [wir-machen-druck.de](https://www.wir-machen-druck.de/extrem-guenstig-Buecher-drucken-lassen,category,15510.html). I've worked with them a couple of times and I am always pleased with both their pricing and quality. Of course other page setups should work fine too and using LaTex this should be a breeze to setup.

I'd love to hear from anyone if they use the code to archive some of their own chats. If you have any questions or comments don't hesitate to drop me a line. As long as I'm not too busy I'd also not mind helping some people eager to give it a go but unfamiliar with python and LaTeX.

I developed this code initially to make a gift, but once finished, I of course discovered people do this for a living. So if you like the idea but are in a hurry or would like a different style maybe look at one of [these](https://www.google.com/search?q=whatsapp+book).