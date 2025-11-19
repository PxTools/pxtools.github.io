# The project New interface for PxWeb (January 2024 - October 2025)

## Background
The user interface in PxWeb "classic" could no longer be effectively developed due to technological limitations, and users needs and expectations are constantly changing so Statistics Sweden and Statistics Norway jointly initiated a project in January 2024. The aim of the project was to create a new user interface based on PxWebApi 2 and was released in its first version in October 2025.
### Goals of the Project 
The project had two goals
- A new user-friendly interface for PxWeb based on PxWebApi 2 
- Test and establish effective and sustainable collaboration processes between Statistics Sweden, SCB and Statistics Norway, SSB at both operational level and management level in the further development of PX-tools
### Organization 
### Steering group
The steering group consist of managers at Communication and IT in SCB and SSB. 
### Project team
The team had participants from both Statistics Sweden and Statistics Norway.
The team was set up with
- Project leader
- Scrum master
- Architects
- UX/UI Designer
- Developers
- Specialists
- Technical lead
- Test leader 
- Testers 
## PxWeb 2 solution
In the project, we have made some choices about how we will work in the project across two countries, in order to create opportunities for other countries to contribute in the long term with both development and user testing of PxWeb 2.0. In addition, we have made some choices to find the best solution for installing PxWeb. All this to reach the main goal to create a user-friendly PxWeb.
### WCAG and Search Engine Optimization (SEO)
- The new interface will follow the rules for A and AA in [WCAG 2.1](https://www.w3.org/TR/WCAG21/)
- SEO is the use of methods that achieve better visibility or "ranking" on search engines result page.
### PxWeb 2.0 designsystem
We build a design system that can be used by all so the same components can be used for all px products so they get a common expression and be recognizable. 
[Design system in Storybook](https://pxweb2.pages.dev/storybook/)
### Options in PxWeb 2.0
Each organization should easily be able to adapt the interface a little, but it should still be possible to see that it is a Px-product. 
The changes will be possible to introduce both before and after installation of PxWeb 2.0.
It will be possible to change
- colors
- font
- corner radius of the elements in the interface.
### Open source at GitHub
The new interface is able to install directly from GitHub. Also documentation is found here. 
[PxTools at GitHub](https://www.pxtools.net/documentation/overview/)
### Usertest
We have been following following the UX design process in this project.
![The Ux design process](https://github.com/PxTools/PxWeb2/assets/81364833/8534bfdb-1d0c-49ef-8f2b-5b59357a37bf)
All the choices we made are based on user insigth. 
The main focus has been navigation, understand the main fuctionaility in PxWeb and how the most used task should work.
We have several usertests during the project, both on desktop and mobile. 
### Possibility for connection
In the PxWeb 2.0 interface, it is easy to connect other tools such as Highcharts, Tableau, Power BI and other AI products. This can be connected both by using saved queries from PxWeb 2.0 or by connecting directly to PxWebApi 2.0.

## Process in the project 
- The project group worked in sprints of three weeks.
- The group follow the scrum rituals with sprintplanning, refinement, retro and daily stand-up.
- The sprints end with a demo (sprint review) on Monday every third week at twelve o'clock.
- Developers have an open Teams-link where they cooperate daily.
- The project group meet physically once a quarter.

## Test tools
| Type of test | Testing tools |
| --- | ----------- |
|Unit and integration tests | Vitest/Jest |
| |Stories in Storybook |
|Static code analysis | CodeQL (in GitHub) |
| Thirdparty code | Dependabot security and version updates,  |
| | Dependabot alerts |
|  | Use Snyk Advisor to check development and maintenance health of libraries |
| Automated end-to-end tests | Mabl|
| Acceptancetests | Mabl, Chromatic: UI Review (Manual test) |
| Regressiontests  | Mabl  |
| |Chromatic: UI Test |
| Accessibility testing |  @storybook/addon-a11ly    Shows relevant results automatically in Storybook,  | 
|  | Axe browser extension |
| |Lighthouse|
| |Mabl with Axe| 
| |Valid HTML5 (W3C validator)  |
| Security tests | [http://securityheaders.io/](http://securityheaders.io/)|
| |Dynamic analysis:  |
| |Detectify | 
| |Nessus  |
| Performance tests | JMeter |
| Container security | Trivy? |
