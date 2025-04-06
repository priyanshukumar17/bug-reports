# User profile picture and name spoofing in comments

## Brief
When adding a comment, the API calls directly to Firebase with the details of the comment and the commentor. This call can be intercepted and custom details can be sent which gets displayed on the website.

## Exploitation
When posting a comment, we can start intercepting the packets and wait for the Firebase API write call which updates the comments for a particular post. We intercept it and check the body.

## Author
<table>
    <tr>
        <td> <img src="https://github.com/bismuth01.png" width="100" style="border-radius: 50%"><br> <strong>Siddhartha Swarnkar</strong> </td>
        <td> <a href="https://github.com/bismuth01"> <img src="https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white"/> </a> <a href="https://x.com/Siddhartha37648"> <img src="https://img.shields.io/badge/Twitter-1DA1F2?style=for-the-badge&logo=twitter&logoColor=white"/> </a> <a href="https://www.linkedin.com/in/siddhartha-swarnkar-704625280/"> <img src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white"/> </a> </td>
    </tr>
</table>
