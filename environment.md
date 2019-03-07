# Setting up your environment

## Python

### Mac OS X

1. Open the Terminal. Just search for Terminal in Finder (`CMD+Space`).
2. You need X-Code, but you might already have it.
   1. Type `xcode-select --install` into the terminal and hit enter.
   2. It should install if it isn't already.
3. Install Homebrew. This is a helpful package manager for OS X.
   1. Head [here](https://brew.sh/) and copy the long command.
   2. Paste into the terminal and press enter.
   3. A bunch of stuff should fly by, just wait for it to complete and check it doesn't mention any errors.
4. Install Python 3.
   1. In the terminal, type `brew install python3` and hit enter.
   2. After a bunch of text flies by, you should have Python 3 installed.
5. [Sign up for a GitHub account](https://github.com/join) if you don't already have one. This may seem a bit overkill but it will pay off in the end. Pick a decent password (or randomly generate one)!
6. Tell me, and I'll add you to the collaborators of the private repositories where the code is stored.
7. [Head to the repo](https://github.com/XavKearney/job-board-scraper) and hit the green **Clone or download** button. Copy the URL.
8. In your terminal, type `git clone https://the-url-you-copied` and hit enter. You'll be prompted for your GitHub username & password.
9. Type `cd job-board-scraper` to enter the folder you just downloaded.
10. Create a new Python virtual environment by typing `pyvenv venv` and hitting enter.
11. Activate the environment with the command `source venv/bin/activate`. That virtual environment will remain active until you close the terminal window.
12. Install the requirements of the program with `pip install -r requirements.txt`.
13. At this point, you're ready to run python scripts by typing `python [name of script]`. In future, just repeat steps 9-11 when you need to run anything.
