## Frone-end Archtectural Design ##

    Date: 04/07/2018
    Author: Jewel / Yu Zhu, Xu
    
What should be considered when designing FE architecture? I think this is an open topic. 
Here's some summary based on my previous development experience.

1. What's your front-end platform? 
    - Web only
    > Make sure how many browsers and their corresponding version.
    - Hybrid
    > ionic, Cordova, react-native
    - Native
    > hmmm, I think we need additional native app developers. lol
    - all of above?
    > Do I need to develop additional projects to support various clients? Or just share one set of code across different platforms.
    
    >Is this framework easy to migrate to mobile projects if we do need?
    
2. How does your front-end communicate with your back-end?
    - HTTP
    - HTTPS
    > You'll need to involve the SSL certificate when setting up the FE web server.
    - WebSocket
    > Consider about the heart beat for stable WebSocket connection.
    
3. Design FE project skeleton per your framework decision. 
    - functional design
    - project structure: startup, core modules, assets, etc

4. HTTP interceptor
    > This is used to set up global error handler for HTTP requests. It can capture error beforehand and expose unified error behavior to endpoint user.

5. Auth Check
    - mem
    - localStorage
    > This will integrate with your back-end authorization method, like OAuth2, JWT, etc.

6. Lazy load
    - AMD
    - CMD
    > This optimize the network level performance, especially the experience on mobile device since usually mobile's hardware is lower than laptop.

7. i18n
    > It's the globalization. I think FED should take this into consideration beforehand.

8. Log mechanism
    - Set up global log utils which allow customization, e.g. level, storage.
    - Customize different log levels for various environments.
        - Development: log, debug, error
        - QA/Staing: debug, error
        - Production: error

9. DevOps
    - devops process
        - bump version
        - separate built files from source code, mkdir `dist`/`build` and copy major files
        - run test cases if exists
        - rebase, update resouce references
        - compile templates if exists
        - compile SCSS/LESS into CSS
        - merge CSS
        - compile JS files
        - merge and obstacure JS files
        - re-index index file
        - build notification
        - provide build command for shell exec
    - customize build arguments for different environments
        - debug arg
        - strick level of JS compile
        - log level
        - test case
