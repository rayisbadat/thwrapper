# TruffleHog_Scripts
A wrapper script around trufflehog to dump finds to a json file and later check against that file. Hopefully can be integrated into travis-ci or jenkins to alert on possible secrets. 

Slowly growing into a huge mess. Will probably rewrite trufflehog to remove that dependency. Trufflehog uses pythonGit which calls subprocess.Popen(git-blah). I think it would be better to use some kind of api if possible. BLAH

— setup —

1.) pip install trufflehog locally

2.) wget my shite script:

     wget https://raw.githubusercontent.com/uc-cdis/thWrapper/master/th_wrapper.py

3.) run it locally to generate your config (may not be required anymore) and your whitelist.

     ./th_wrapper.py -w truffles.json -c thog_config.json -g file:///local/path/to/repo --cinit
     ./th_wrapper.py -w truffles.json -c thog_config.json -g file:///local/path/to/repo --init
     
4.) Add the truffles.json and thog_config.json to your git repo

5.) add thog_secret_requirements.txt to your repo:

     truffleHog>=2.0.97
     truffleHogRegexes>=0.0.6
     PyGithub==1.43.2
     
add the following snippet to .travis.yaml under "before_install:"

     - pip install -r thog_requirements.txt
     - wget https://raw.githubusercontent.com/uc-cdis/thWrapper/master/th_wrapper.py
     - python th_wrapper.py -w truffles.json -c thog_config.json -g file:///${PWD}/ --check
     
from now on any future commit is checked against that whitelist. 

to blanket add new all new found secrets to your whitelist:

     ./th_wrapper.py -w truffles.json -c thog_config.json -g file:///local/path/to/repo  --init

to scan and prompt for all new secrets before adding to whitelist

     ./th_wrapper.py -w truffles.json -c thog_config.json -g file:///local/path/to/repo
     
to scan and error on new findings add --check

I'm working on a pre-commit hook now as the other half of this but considering most people wont use the pre-commit hook. I figured this would do. 


