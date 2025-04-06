# User profile picture and name spoofing in comments

## Brief
When adding a comment, the API calls directly to Firebase with the details of the comment and the commentor. This call can be intercepted and custom details can be sent which gets displayed on the website.

## Exploitation
When posting a comment, we can start intercepting the packets and wait for the Firebase API write call which updates the comments for a particular post. We intercept it and check the body parameters. In the `req0__data_`, we get all the json data, here we can change a multitude of parameter. In`author`, changing `stringValue` changes the name displayed on the comment. In `authorPhotoURL`, changing `stringValue` changes the link to the profile photo of the comment.

#### Proof of Concept
![Video showing changing of username on comment](https://github.com/bismuth01/bug-reports/blob/main/LNMDoubts/comment_user_info_spoofing/Comments_username_spoofing.mp4)
![Video showing changing of profile picture on comment](https://github.com/bismuth01/bug-reports/blob/main/LNMDoubts/comment_user_info_spoofing/Commnet_profile_pic_upload.mp4)

## CVSS Score
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
      <td style="padding: 8px;">6.8</td>
    </tr>
    <tr>
      <td style="padding: 8px;">Severity</td>
      <td style="padding: 8px;">Medium</td>
    </tr>
    <tr>
      <td style="padding: 8px;">Vector String</td>
      <td style="padding: 8px;"><code>CVSS:3.1/AV:N/AC:L/PR:N/UI:R/S:U/C:L/I:H/A:N</code></td>
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
      <td style="padding: 8px;">Required</td>
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

<p><a href="https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:L/PR:N/UI:R/S:U/C:L/I:H/A:N">🔗 Open in CVSS Calculator</a></p>


## Root cause
Whenever a comment is added, a Firebase API request is sent directly to the Firebase database to append the comment data to the post. This request can intercepted and any of the values can be changed. Due to no checking on the backend, it is accepted and added. Whenever the post is rendered, likely, the database data is directly accessed and added to the DOM.

## Recovery
Since the data is directly added, it can be easily removed from the list of comments for the post in the database.

## Prevention
Instead of taking all the data to be displayed, it is better to take one identifying token of the user and then fetch for the details of the user through that token and input it into the database. Such a function can be easily setup with a backend server.
If a custom data section is required, strict checking should be enabled in the database side to check if the URL is of an image and valid and the username is a valid one too. This can be setup in Firebase through rules.

## Author
<table>
    <tr>
        <td> <img src="https://github.com/bismuth01.png" width="100" style="border-radius: 50%"><br> <strong>Siddhartha Swarnkar</strong> </td>
        <td> <a href="https://github.com/bismuth01"> <img src="https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white"/> </a> <a href="https://x.com/Siddhartha37648"> <img src="https://img.shields.io/badge/Twitter-1DA1F2?style=for-the-badge&logo=twitter&logoColor=white"/> </a> <a href="https://www.linkedin.com/in/siddhartha-swarnkar-704625280/"> <img src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white"/> </a> </td>
    </tr>
</table>
