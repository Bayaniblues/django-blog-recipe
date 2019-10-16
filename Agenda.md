The over all plan for the Django Voice project is to connect a powerful Django backend to a simple voice interface, 
like voiceflow. These recipe guides are built from Antonio Mele's book Django 2 by example, and from the django documentation. 

Projects
---
Hacked Headlines (in development)
    
    Technologies used:
        - Voiceflow
        - django REST for read only Json data
        - airtable as a cheap way to store results.
    Process:
        1. Scrape 1 million headlines from Hacker news
        2. Train GPT-2 on these headlines
        3. Filter the Headlines into a JSON array
        4. Make a turning test game on Voiceflow
        5. Deploy 

poetbot (scraping from Gutenburg)

    - Voiceflow for fake poetry game, and Local storage
    - Django for SMTP email and landing page
    - Postgresql for userstorage, and for storing poetry lines.
    
DjangoVoice (studying)

    - Make django blog
    - Enable email SMTP
    
Make promotional video in Blender (studying youtube videos)

    Make 3d fonts
    flat PNG's
    Enable gravity 
    Fonts bend and rotate in circular motion
    Amazon polly voiceover
    
Make Goalline (studying matplotlib)
    
    = PROTOTYPING PHASE =
    - Create interactive calendar in Matplotlib
    - Make Matplotlib interact with django REST
    - Connect Voiceflow and django backend.

TODO:Create Useful broiler plates
--

The voice blog for business and charity(easy)


    A Blog website that connects to Voiceflow via the REST API or RSS feed. With SMTP emailing.
    Charities can use this to get subscriptions via ISP's
    Business can extend their brand and gain leads.
    
User management(medium)

Connecting alexa's user_id through account linking to Django's rest API through postgresql sounds hard,
lets break it down.
    
    1. In voiceflow, enable account linking
    2. get user_id
    3. store user_id and email in postgresql through Django's rest API
    4. Accounts should be created and synced from website2alexa, or alexa2website
    5. Check for any security Vulnerabilities with this method
    
This approach sounds difficult, but it will provide Opportunities to create voice based enterprises. 

Ecommerce(Hard)

    The goal is to enable ecommerce through voice and ISP. This would probably work well with Django rest API, 
    On the bright side, this can bypass having a amazon's seller's account and sellers can receive product orders though 
    amazon... in the not.so.bright side, amazon might not like us bypassing their seller's central, and sellers might not 
    like prime members purchasing items with their discount... lets build it and find out if it gets approved!


social media(hardest)

    Create a social media platform for alexa, is possible with django, I will revisit this idea in the future




