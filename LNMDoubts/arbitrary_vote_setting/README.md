# Setting Arbitrary number of votes in a post

## Brief
The API call to Firebase to directly set the number of current votes on a post, leads to updating the votes to any number.

## Exploitation
When the upvote or downvote button is clicked, the intercept of packets is started. Once the data update call of the Firebase API comes up, the `req0__data_` parameter of the body can be opened. It is a URL encoded json, where inside the `total_votes` field, we update the value of `integerValue` to any number of votes we want.

#### Proof of Concept
![Showing how packet is intercepted and exploitated](https://github.com/bismuth01/bug-reports/blob/main/LNMDoubts/arbitrary_vote_setting/Votes_setting.mp4)

## CVSS Score Summary
<h3>🧮 CVSS Score Summary</h3>

<table style="border-collapse: collapse; width: 100%;">
  <thead>
    <tr style="background-color: #f2f2f2;">
      <th style="text-align: left; padding: 8px;">Metric</th>
      <th style="text-align: left; padding: 8px;">Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="padding: 8px;">CVSS Score</td>
      <td style="padding: 8px;">7.5</td>
    </tr>
    <tr>
      <td style="padding: 8px;">Severity</td>
      <td style="padding: 8px;">High</td>
    </tr>
    <tr>
      <td style="padding: 8px;">Vector String</td>
      <td style="padding: 8px;"><code>CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:L/I:H/A:N</code></td>
    </tr>
    <tr>
      <td style="padding: 8px;">Attack Vector</td>
      <td style="padding: 8px;">Network</td>
    </tr>
    <tr>
      <td style="padding: 8px;">Attack Complexity</td>
      <td style="padding: 8px;">Low</td>
    </tr>
    <tr>
      <td style="padding: 8px;">Privileges Required</td>
      <td style="padding: 8px;">None</td>
    </tr>
    <tr>
      <td style="padding: 8px;">User Interaction</td>
      <td style="padding: 8px;">None</td>
    </tr>
    <tr>
      <td style="padding: 8px;">Scope</td>
      <td style="padding: 8px;">Unchanged</td>
    </tr>
    <tr>
      <td style="padding: 8px;">Confidentiality Impact</td>
      <td style="padding: 8px;">Low</td>
    </tr>
    <tr>
      <td style="padding: 8px;">Integrity Impact</td>
      <td style="padding: 8px;">High</td>
    </tr>
    <tr>
      <td style="padding: 8px;">Availability Impact</td>
      <td style="padding: 8px;">None</td>
    </tr>
  </tbody>
</table>

<p><a href="https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:L/I:H/A:N">🔗 Open in CVSS Calculator</a></p>

## Root Cause
The cause is that everytime a vote is added, the number of total votes is re-written directly through the Firebase API call. No checks on the server side are done to make sure the number changes are done in a valid or authorized manner.

## Recovery
Since, the total number of votes are directly re-written, can only be recovered through searching through the logs and manually chaning them.

## Prevention
Rather than directly updating the database, it is recommended to make custom actions for upvote / downvote to in-turn update the value of total number of posts, to prevent unauthorized update or control. This can be done by creating a simple backend with CORS enabled for another layer of protection.
Another way to protect is set up a rule in Firebase database to prevent any updates more than +1 or -1 to the number of votes to a post.

## Author
<table>
    <tr>
        <td> <img src="https://github.com/bismuth01.png" width="100" style="border-radius: 50%"><br> <strong>Siddhartha Swarnkar</strong> </td>
        <td> <a href="https://github.com/bismuth01"> <img src="https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white"/> </a> <a href="https://x.com/Siddhartha37648"> <img src="https://img.shields.io/badge/Twitter-1DA1F2?style=for-the-badge&logo=twitter&logoColor=white"/> </a> <a href="https://www.linkedin.com/in/siddhartha-swarnkar-704625280/"> <img src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white"/> </a> </td>
    </tr>
</table>
