<div id="top"></div>

<!-- PROJECT SHIELDS -->
<!--
*** I'm using markdown "reference style" links for readability.
*** Reference links are enclosed in brackets [ ] instead of parentheses ( ).
*** See the bottom of this document for the declaration of the reference variables
*** for contributors-url, forks-url, etc. This is an optional, concise syntax you may use.
*** https://www.markdownguide.org/basic-syntax/#reference-style-links
-->
<!-- [![Contributors][contributors-shield]][contributors-url]
[![Forks][forks-shield]][forks-url]
[![Stargazers][stars-shield]][stars-url]
[![Issues][issues-shield]][issues-url]
[![MIT License][license-shield]][license-url]
[![LinkedIn][linkedin-shield]][linkedin-url] -->



<!-- PROJECT LOGO -->
<!-- <br />
<div align="center">
  <a href="https://github.com/github_username/repo_name">
    <img src="images/logo.png" alt="Logo" width="80" height="80">
  </a> -->

<h3 align="center">project_title</h3>

  <p align="center">
    A simple workaround to allow Audinate's DVS to work more reliably on Apple M1s
    <br />
    <a href="https://github.com/bam-studios/dvs-reset-tools"><strong>Explore the docs »</strong></a>
    <br />
    <br />
    <a href="https://github.com/bam-studios/dvs-reset-tools/issues">Report Bug</a>
    ·
    <a href="https://github.com/bam-studios/dvs-reset-tools/issues">Request Feature</a>
  </p>
</div>



<!-- TABLE OF CONTENTS -->



<!-- ABOUT THE PROJECT -->
## About The Project

<!-- [![Product Name Screen Shot][product-screenshot]](https://example.com) -->

<!-- Here's a blank template to get started: To avoid retyping too much info. Do a search and replace with your text editor for the following: `github_username`, `repo_name`, `twitter_handle`, `linkedin_username`, `email_client`, `email`, `project_title`, `project_description` -->

<p align="right">(<a href="#top">back to top</a>)</p>



### Built With

<p align="right">(<a href="#top">back to top</a>)</p>



<!-- GETTING STARTED -->
## Getting Started

This is an example of how you may give instructions on setting up your project locally.
To get a local copy up and running follow these simple example steps.

### Prerequisites

This is an example of how to list things you need to use the software and how to install them.
* npm
  ```sh
  npm install npm@latest -g
  ```

### Installation

1. Get a free API Key at [https://example.com](https://example.com)
2. Clone the repo
   ```sh
   git clone https://github.com/github_username/repo_name.git
   ```

<p align="right">(<a href="#top">back to top</a>)</p>



<!-- USAGE EXAMPLES -->
## Usage

Use this space to show useful examples of how a project can be used. Additional screenshots, code examples and demos work well in this space. You may also link to more resources.

_For more examples, please refer to the [Documentation](https://example.com)_

<p align="right">(<a href="#top">back to top</a>)</p>



<!-- ROADMAP -->



<!-- CONTRIBUTING -->
## Contributing

Contributions are what make the open source community such an amazing place to learn, inspire, and create. Any contributions you make are **greatly appreciated**.

If you have a suggestion that would make this better, please fork the repo and create a pull request. You can also simply open an issue with the tag "enhancement".
Don't forget to give the project a star! Thanks again!

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

<p align="right">(<a href="#top">back to top</a>)</p>



<!-- LICENSE -->
## License

Distributed under the MIT License. See `LICENSE.txt` for more information.

<p align="right">(<a href="#top">back to top</a>)</p>



<!-- CONTACT -->
## Contact

Michael Check - mcheck@bamstudios.com

Project Link: [https://github.com/bam-studios/dvs-reset-tools](https://github.com/bam-studios/dvs-reset-tools)

<p align="right">(<a href="#top">back to top</a>)</p>



<!-- ACKNOWLEDGMENTS -->
## Acknowledgments

* []()

<p align="right">(<a href="#top">back to top</a>)</p>



<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->
<!-- [contributors-shield]: https://img.shields.io/github/contributors/github_username/repo_name.svg?style=for-the-badge
[contributors-url]: https://github.com/github_username/repo_name/graphs/contributors
[forks-shield]: https://img.shields.io/github/forks/github_username/repo_name.svg?style=for-the-badge
[forks-url]: https://github.com/github_username/repo_name/network/members
[stars-shield]: https://img.shields.io/github/stars/github_username/repo_name.svg?style=for-the-badge
[stars-url]: https://github.com/github_username/repo_name/stargazers
[issues-shield]: https://img.shields.io/github/issues/github_username/repo_name.svg?style=for-the-badge
[issues-url]: https://github.com/github_username/repo_name/issues
[license-shield]: https://img.shields.io/github/license/github_username/repo_name.svg?style=for-the-badge
[license-url]: https://github.com/github_username/repo_name/blob/master/LICENSE.txt
[linkedin-shield]: https://img.shields.io/badge/-LinkedIn-black.svg?style=for-the-badge&logo=linkedin&colorB=555
[linkedin-url]: https://linkedin.com/in/linkedin_username
[product-screenshot]: images/screenshot.png -->




## Instructions
**Description:**
DVS LaunchDaemon lives in `/Library/LaunchDaemons`
With Apple M1 systems, we've found DVS does not always correctly start up at boot. Intermittently, DVS cannot find the network. In order to accommodate finding the network on DVS startup, we delay the daemon start and changed the deprecated command to use the current `KeepAlive`.



## DVS not running as expected
If DVS is not running correctly, first try to UNLOAD and RELOAD the DVS DAEMON:

### Unload the daemon

1. Quit DVS if it is running.

1. Double click on the application DVS-UNLOAD-DAEMON.app

1. You will be asked to enter an admin password and possibly verify access to System Events.

1. There is no visual feedback. The daemon will be unloaded. An attempt to run DVS will result in a missing Dante connection.

#### Equivalent running from terminal

```
sudo launchctl unload -w /Library/LaunchDaemons/com.audinate.dante.DanteVirtualSoundcard.plist
```


### Load the Daemon

1. Quit DVS if it is running.

1. Double click on the application DVS-LOAD-DAEMON.app

1. You will be asked to enter an admin password and possibly verify access to System Events.

1. There is no visual feedback. The daemon will be loaded.

1. Open Dante Virtual Soundcard

#### Equivalent running from terminal

```
sudo launchctl load -w /Library/LaunchDaemons/com.audinate.dante.DanteVirtualSoundcard.plist
```


## Modify or restore the DVS LaunchDaemon

Add a `nice` delay to the startup daemon so it waits for the network to start.
`nice` is a LaunchDaemon supported program that takes an integer value between -20 and 20, 
The LaunchDaemon files can be restored to original or modified with delay and non-deprecated commands.


### Modify DVS LaunchDaemon process to delayed start and KeepAlive

To install the modified DVS LaunchDaemon with non-deprecated commands and a delayed start:

1. Open the terminal
1. Run
```
sudo LaunchDaemons/com.audinate.dante.DanteVirtualSoundcard.plist.modified /Library/LaunchDaemons/com.audinate.dante.DanteVirtualSoundcard.plist
```
1. Enter admin password when prompted.
1. Close terminal
1. Run the UNLOAD/LOAD process above


### Restore LaunchDaemon process to original

To restore the DVS LaunchDaemon as installed from Audinate:

1. Open the terminal
1. Run
```
sudo cp LaunchDaemons/com.audinate.dante.DanteVirtualSoundcard.plist.original /Library/LaunchDaemons/com.audinate.dante.DanteVirtualSoundcard.plist
```

1. Enter admin password when prompted.
1. Close terminal
1. Run the UNLOAD/LOAD process above
