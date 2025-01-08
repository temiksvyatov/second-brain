---
date:
  - 13/05/2024 20:36
tags:
  - guide
  - code
cssclasses:
  - image-borders
  - neutral-pen-black
  - page-manila
---
# [Code Review Best Practices For Software Engineers](https://youtu.be/1Ge__2Yx_XQ?si=RbUolklsx9xqwZ_e)
<iframe width="560" height="315" src="https://www.youtube.com/embed/1Ge__2Yx_XQ?si=_AiAxZTb90Ihd_O7" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

> The goal of code review is <mark style="background: #D2B3FFA6;">NOT ONLY</mark> to fix bugs or show the developer their mistakes but also to achieve the state of the process when it becomes frictionless and seamless.

## Tips for submitting the code
### Tip 1: Send as ==*less code*== as possible
The thousands lines of code on a code review are a PIA for a code reviewer cause it makes the process a nightamre. One of the basic reasons for this is the difficulty of <mark style="background: #D2B3FFA6;">cathcing the bugs</mark> during the analysis.
> The author says to keep PR/MR between `10-100` lines of code.

### Tip 2: Leave an informative description of MR
Reviewer will keep some context about your changes and updates in code during the review. ==It will prevent from unceartin treads about meaning and usability of written code.==
It’s a good practice to show as much clear information as possible. It might be:
- A simple line `fix: add auth method`
- A complex implementation the requires a comprehensive description
- A link to a task in Task Tracker (i.e. *Notion*)
- A screenshot of before and after changes
- <mark style="background: #D2B3FFA6;">A prompt that generated the code</mark>
### Tip 3: Comment on neccessary parts of diffs that appeared in MR
Better to leave the comment in a code where some changes happend so code reviewer knows that the changes are might not be obvious but still usefull in this MR. ==So the comments are NOT for a code base but for a code reviewer.==

## Tips for reviewing the code
### Tip 1: Leave clear comments
The important part of code review is <mark style="background: #D2B3FFA6;">an explicit communication between reciever and sender</mark>. Comments may not only be critical by pointing to coder mistakes. They allow to show different feedback based on a situation. You may leave a comment to:
- Point to mistakes and bottle neks
- Suggest changes and personal views. The reviewer can ask for cahnges and also allow to make a decition by developer.
- Ask questions to clarify some parts of code
### Tip 2: Set a list of rules when to approve/disapprove the MR
==The person who sends an MR should be aware of reasons why their code was not approved by a reviewer.== In other way it’s just a subjective POV withour any basic requirements to code.
### Tip 3: decide when to leave comments and when to have a live conversation
There might be situation when i.e. the whole written code should be rewritten. In this case it’s better not to wrtite the paragraphs of comments and play a ping-pong chatting in threads with developer but to meet a developer offline and discuss things together.
### Additional Tip (good for both): it’s okay to disagree with comment
If you are not agree with left comment suggestions, just leave an answers describing the reason of disagreement.