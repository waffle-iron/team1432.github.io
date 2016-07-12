# FRC Team 1432 Blog Website: [team1432.github.io](https://team1432.github.io)
[![Travis status](https://img.shields.io/travis/team1432/team1432.github.io/master.svg?maxAge=2592000&label=build%20status)](https://travis-ci.org/team1432/team1432.github.io)
[![GitHub issues](https://img.shields.io/github/issues/team1432/team1432.github.io.svg?maxAge=2592000&label=github%20issues&logo=data:image/svg%2Bxml%3Bbase64%2CPD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiIHN0YW5kYWxvbmU9Im5vIj8%2BDQo8c3ZnIHdpZHRoPSIyNTZweCIgaGVpZ2h0PSIyNTBweCIgdmlld0JveD0iMCAwIDI1NiAyNTAiIHZlcnNpb249IjEuMSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB4bWxuczp4bGluaz0iaHR0cDovL3d3dy53My5vcmcvMTk5OS94bGluayIgcHJlc2VydmVBc3BlY3RSYXRpbz0ieE1pZFlNaWQiPg0KICAgIDxnPg0KICAgICAgICA8cGF0aCBkPSJNMTI4LjAwMTA2LDAgQzU3LjMxNzI5MjYsMCAwLDU3LjMwNjY5NDIgMCwxMjguMDAxMDYgQzAsMTg0LjU1NTI4MSAzNi42NzYxOTk3LDIzMi41MzU1NDIgODcuNTM0OTM3LDI0OS40NjA4OTkgQzkzLjkzMjAyMjMsMjUwLjY0NTc3OSA5Ni4yODA1ODgsMjQ2LjY4NDE2NSA5Ni4yODA1ODgsMjQzLjMwMzMzMyBDOTYuMjgwNTg4LDI0MC4yNTEwNDUgOTYuMTYxODg3OCwyMzAuMTY3ODk5IDk2LjEwNjc3NywyMTkuNDcyMTc2IEM2MC40OTY3NTg1LDIyNy4yMTUyMzUgNTIuOTgyNjIwNywyMDQuMzY5NzEyIDUyLjk4MjYyMDcsMjA0LjM2OTcxMiBDNDcuMTU5OTU4NCwxODkuNTc0NTk4IDM4Ljc3MDQwOCwxODUuNjQwNTM4IDM4Ljc3MDQwOCwxODUuNjQwNTM4IEMyNy4xNTY4Nzg1LDE3Ny42OTYxMTMgMzkuNjQ1ODIwNiwxNzcuODU5MzI1IDM5LjY0NTgyMDYsMTc3Ljg1OTMyNSBDNTIuNDk5MzQxOSwxNzguNzYyMjkzIDU5LjI2NzM2NSwxOTEuMDQ5ODcgNTkuMjY3MzY1LDE5MS4wNDk4NyBDNzAuNjgzNzY3NSwyMTAuNjE4NDIzIDg5LjIxMTU3NTMsMjA0Ljk2MTA5MyA5Ni41MTU4Njg1LDIwMS42OTA0ODIgQzk3LjY2NDcxNTUsMTkzLjQxNzUxMiAxMDAuOTgxOTU5LDE4Ny43NzA3OCAxMDQuNjQyNTgzLDE4NC41NzQzNTcgQzc2LjIxMTc5OSwxODEuMzM3NjYgNDYuMzI0ODE5LDE3MC4zNjIxNDQgNDYuMzI0ODE5LDEyMS4zMTU3MDIgQzQ2LjMyNDgxOSwxMDcuMzQwODg5IDUxLjMyNTA1ODgsOTUuOTIyMzY4MiA1OS41MTMyNDM3LDg2Ljk1ODM5MzcgQzU4LjE4NDIyNjgsODMuNzM0NDE1MiA1My44MDI5MjI5LDcwLjcxNTU2MiA2MC43NTMyMzU0LDUzLjA4NDM2MzYgQzYwLjc1MzIzNTQsNTMuMDg0MzYzNiA3MS41MDE5NTAxLDQ5LjY0NDE4MTMgOTUuOTYyNjQxMiw2Ni4yMDQ5NTk1IEMxMDYuMTcyOTY3LDYzLjM2ODg3NiAxMTcuMTIzMDQ3LDYxLjk0NjU5NDkgMTI4LjAwMTA2LDYxLjg5Nzg0MzIgQzEzOC44NzkwNzMsNjEuOTQ2NTk0OSAxNDkuODM3NjMyLDYzLjM2ODg3NiAxNjAuMDY3MDMzLDY2LjIwNDk1OTUgQzE4NC40OTgwNSw0OS42NDQxODEzIDE5NS4yMzE5MjYsNTMuMDg0MzYzNiAxOTUuMjMxOTI2LDUzLjA4NDM2MzYgQzIwMi4xOTkxOTcsNzAuNzE1NTYyIDE5Ny44MTU3NzMsODMuNzM0NDE1MiAxOTYuNDg2NzU2LDg2Ljk1ODM5MzcgQzIwNC42OTQwMTgsOTUuOTIyMzY4MiAyMDkuNjYwMzQzLDEwNy4zNDA4ODkgMjA5LjY2MDM0MywxMjEuMzE1NzAyIEMyMDkuNjYwMzQzLDE3MC40Nzg3MjUgMTc5LjcxNjEzMywxODEuMzAzNzQ3IDE1MS4yMTMyODEsMTg0LjQ3MjYxNCBDMTU1LjgwNDQzLDE4OC40NDQ4MjggMTU5Ljg5NTM0MiwxOTYuMjM0NTE4IDE1OS44OTUzNDIsMjA4LjE3NjU5MyBDMTU5Ljg5NTM0MiwyMjUuMzAzMzE3IDE1OS43NDY5NjgsMjM5LjA4NzM2MSAxNTkuNzQ2OTY4LDI0My4zMDMzMzMgQzE1OS43NDY5NjgsMjQ2LjcwOTYwMSAxNjIuMDUxMDIsMjUwLjcwMDg5IDE2OC41MzkyNSwyNDkuNDQzOTQxIEMyMTkuMzcwNDMyLDIzMi40OTk1MDcgMjU2LDE4NC41MzYyMDQgMjU2LDEyOC4wMDEwNiBDMjU2LDU3LjMwNjY5NDIgMTk4LjY5MTE4NywwIDEyOC4wMDEwNiwwIFogTTQ3Ljk0MDU1OTMsMTgyLjM0MDIxMiBDNDcuNjU4NjQ2NSwxODIuOTc2MTA1IDQ2LjY1ODE3NDUsMTgzLjE2Njg3MyA0NS43NDY3Mjc3LDE4Mi43MzAyMjcgQzQ0LjgxODMyMzUsMTgyLjMxMjY1NiA0NC4yOTY4OTE0LDE4MS40NDU3MjIgNDQuNTk3ODgwOCwxODAuODA3NzEgQzQ0Ljg3MzQzNDQsMTgwLjE1MjczOSA0NS44NzYwMjYsMTc5Ljk3MDQ1IDQ2LjgwMjMxMDMsMTgwLjQwOTIxNiBDNDcuNzMyODM0MiwxODAuODI2Nzg2IDQ4LjI2Mjc0NTEsMTgxLjcwMjE5OSA0Ny45NDA1NTkzLDE4Mi4zNDAyMTIgWiBNNTQuMjM2Nzg5MiwxODcuOTU4MjU0IEM1My42MjYzMzE4LDE4OC41MjQxOTkgNTIuNDMyOTcyMywxODguMjYxMzYzIDUxLjYyMzI2ODIsMTg3LjM2Njg3NCBDNTAuNzg2MDA4OCwxODYuNDc0NTA0IDUwLjYyOTE1NTMsMTg1LjI4MTE0NCA1MS4yNDgwOTEyLDE4NC43MDY3MiBDNTEuODc3NjI1NCwxODQuMTQwNzc1IDUzLjAzNDk1MTIsMTg0LjQwNTczMSA1My44NzQzMzAyLDE4NS4yOTgxMDEgQzU0LjcxMTU4OTIsMTg2LjIwMTA2OSA1NC44NzQ4MDE5LDE4Ny4zODU5NSA1NC4yMzY3ODkyLDE4Ny45NTgyNTQgWiBNNTguNTU2MjQxMywxOTUuMTQ2MzQ3IEM1Ny43NzE5NzMyLDE5NS42OTEwOTYgNTYuNDg5NTg4NiwxOTUuMTgwMjYxIDU1LjY5Njg0MTcsMTk0LjA0MjAxMyBDNTQuOTEyNTczMywxOTIuOTAzNzY0IDU0LjkxMjU3MzMsMTkxLjUzODcxMyA1NS43MTM3OTksMTkwLjk5MTg0NSBDNTYuNTA4NjY1MSwxOTAuNDQ0OTc3IDU3Ljc3MTk3MzIsMTkwLjkzNjczNSA1OC41NzUzMTgxLDE5Mi4wNjY1MDUgQzU5LjM1NzQ2NjksMTkzLjIyMzgzIDU5LjM1NzQ2NjksMTk0LjU4ODg4IDU4LjU1NjI0MTMsMTk1LjE0NjM0NyBaIE02NS44NjEzNTkyLDIwMy40NzExNzQgQzY1LjE1OTc1NzEsMjA0LjI0NDg0NiA2My42NjU0MDgzLDIwNC4wMzcxMiA2Mi41NzE2NzE3LDIwMi45ODE1MzggQzYxLjQ1MjQ5OTksMjAxLjk0OTI3IDYxLjE0MDkxMjIsMjAwLjQ4NDU5NiA2MS44NDQ2MzQxLDE5OS43MTA5MjYgQzYyLjU1NDcxNDYsMTk4LjkzNTEzNyA2NC4wNTc1NDIyLDE5OS4xNTM0NiA2NS4xNTk3NTcxLDIwMC4yMDA1NjQgQzY2LjI3MDQ1MDYsMjAxLjIzMDcxMiA2Ni42MDk1OTM2LDIwMi43MDU5ODQgNjUuODYxMzU5MiwyMDMuNDcxMTc0IFogTTc1LjMwMjUxNTEsMjA2LjI4MTU0MiBDNzQuOTkzMDQ3NCwyMDcuMjg0MTM0IDczLjU1MzgwOSwyMDcuNzM5ODU3IDcyLjEwMzk3MjQsMjA3LjMxMzgwOSBDNzAuNjU2MjU1NiwyMDYuODc1MDQzIDY5LjcwODc3NDgsMjA1LjcwMDc2MSA3MC4wMDEyODU3LDIwNC42ODc1NzEgQzcwLjMwMjI3NSwyMDMuNjc4NjIxIDcxLjc0Nzg3MjEsMjAzLjIwMzgyIDczLjIwODMwNjksMjAzLjY1OTU0MyBDNzQuNjUzOTA0MSwyMDQuMDk2MTkgNzUuNjAzNTA0OCwyMDUuMjYxOTk0IDc1LjMwMjUxNTEsMjA2LjI4MTU0MiBaIE04Ni4wNDY5NDcsMjA3LjQ3MzYyNyBDODYuMDgyOTgwNiwyMDguNTI5MjA5IDg0Ljg1MzU4NzEsMjA5LjQwNDYyMiA4My4zMzE2ODI5LDIwOS40MjM3IEM4MS44MDEzLDIwOS40NTc2MTQgODAuNTYzNDI4LDIwOC42MDMzOTggODAuNTQ2NDcwOCwyMDcuNTY0NzcyIEM4MC41NDY0NzA4LDIwNi40OTg1OTEgODEuNzQ4MzA4OCwyMDUuNjMxNjU3IDgzLjI3ODY5MTcsMjA1LjYwNjIyMSBDODQuODAwNTk2MiwyMDUuNTc2NTQ2IDg2LjA0Njk0NywyMDYuNDI0NDAzIDg2LjA0Njk0NywyMDcuNDczNjI3IFogTTk2LjYwMjE0NzEsMjA3LjA2OTAyMyBDOTYuNzg0NDM2NiwyMDguMDk5MTcxIDk1LjcyNjczNDEsMjA5LjE1Njg3MiA5NC4yMTU0MjgsMjA5LjQzODc4NSBDOTIuNzI5NTU3NywyMDkuNzEwMDk5IDkxLjM1MzkwODYsMjA5LjA3NDIwNiA5MS4xNjUyNjAzLDIwOC4wNTI1MzggQzkwLjk4MDg1MTUsMjA2Ljk5Njk1NSA5Mi4wNTc2MzA2LDIwNS45MzkyNTMgOTMuNTQxMzgxMywyMDUuNjY1ODIgQzk1LjA1NDgwNywyMDUuNDAyOTg0IDk2LjQwOTI1OTYsMjA2LjAyMTkxOSA5Ni42MDIxNDcxLDIwNy4wNjkwMjMgWiIgZmlsbD0iI0ZGRkZGRiI%2BPC9wYXRoPg0KICAgIDwvZz4NCjwvc3ZnPg%3D%3D)](https://github.com/team1432/team1432.github.io/issues)


## Set Up

1. Create a [**GitHub account**](https://github.com/)
2. Ask me ([@calebeby](https://github.com/calebeby)) to give you write access to the repository
2. Go to **[Prose.io](http://prose.io)** and click on the **Authorize on Github** button

  ![Authorize](https://github.com/team1432/team1432.github.io/raw/source/source/images/README/authorize.png)

3. **Sign in** with your GitHub account that you made before

That's it!

## Create a Post

1. Go to **[Trello](https://trello.com/b/iLpXk1W6/post-ideas)** to get a post idea

  > **NOTE:** You must be signed in
  
2. Go to **[Prose.io](http://prose.io)** and click on **team1432.github.io**
3. Click **`New Post`**
4. Add a title
5. Fill in the author and tags information

  ![Metadata Button](https://github.com/team1432/team1432.github.io/raw/source/source/images/README/metadata.png)

6. Follow the instructions for editing a post

## Editing a Post

1. Go to **[Prose.io](http://prose.io)** and click on **team1432.github.io**
2. Click on the post you would like to edit
3. Edit the post content
4. Edit the post information by clicking on the metadata button on the right:

  ![Metadata Button](https://github.com/team1432/team1432.github.io/raw/source/source/images/README/metadata.png)

5. When you're done, click on the **save** button on the right:

  ![Save Button](https://github.com/team1432/team1432.github.io/raw/source/source/images/README/save.png)

6. Click **Commit**
7. Go to **[Travis CI](https://travis-ci.org/team1432/team1432.github.io)**
8. Wait until it goes **green** or **red**. If it goes green, it passed. If it goes red, it means there was an error. Scroll through the log to try to figure out what went wrong. It **will fail** if there are spelling errors.

## Publising a Post (Making it visible on <http://team1432.github.io/blog>)

1. Go to **[Prose.io](http://prose.io)** and click on **team1432.github.io**
2. Click on the post you would like to publish
3. Re-read the post content.
4. Check the post information by clicking on the metadata button on the right:

  ![Metadata Button](https://github.com/team1432/team1432.github.io/raw/source/source/images/README/metadata.png)
  
  Make sure that the date, author, and tags are correct.

5. Go back to edit mode

  ![Edit Button](https://github.com/team1432/team1432.github.io/raw/source/source/images/README/edit.png)
  
6. Click on `Unpublished` to publish

  ![Publish Button](https://github.com/team1432/team1432.github.io/raw/source/source/images/README/publish.png)

5. Click on the **save** button on the right:

  ![Save Button](https://github.com/team1432/team1432.github.io/raw/source/source/images/README/save.png)

6. Click **Commit**
7. Go to **[Travis CI](https://travis-ci.org/team1432/team1432.github.io)**
8. Wait until it goes **green** or **red**. If it goes green, it passed. If it goes red, it means there was an error. Scroll through the log to try to figure out what went wrong. It **will fail** if there are spelling errors. If you are *sure* that the word is spelled correctly, you can add it to the list on [Prose.io](http://prose.io/#team1432/team1432.github.io/edit/source/spelling.txt)
