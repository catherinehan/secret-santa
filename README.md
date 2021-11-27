Intro
=====

**secret-santa** can help you manage a list of secret santa participants by
randomly assigning pairings and sending emails. It can avoid pairing 
couples to their significant other, and allows custom email messages to be 
specified.

Modified from original source:
https://github.com/underbluewaters/secret-santa

Dependencies
------------

pytz
pyyaml

Usage
-----

Copy config.yml.template to config.yml and enter in the connection details 
for your outgoing mail server. Modify the participants and couples lists and 
the email message if you wish.

    cd secret-santa/
    cp config.yml.template config.yml

Here is the example configuration unchanged:

    # Required to connect to your outgoing mail server. Example for using gmail:
    # gmail
    SMTP\_SERVER: smtp.gmail.com
    SMTP\_PORT: 587
    USERNAME: you@gmail.com
    PASSWORD: "your-password"
    
    TIMEZONE: 'US/Pacific'
    
    # name, email, hobbies, mailing address delimited by "|"
    # The delimiter "|" should not appear elsewhere in the string
    PARTICIPANTS:
      - foo|foo@somewhere.net|hobbies|address
      - bar|bar@somewhere.net|hobbies2|address2
      - baz|baz@somewhere.net|hobbies3|address3
      - garply|garply@somewhere.net|hobbies4|address4
    
    DONT-PAIR:
      - foo, bar    # foo and bar are married
    
    # From address should be the organizer in case participants have any questions
    FROM: You <you@gmail.net>
    
    # Both SUBJECT and MESSAGE can include variable substitution for the 
    # "santa" and "santee"
    SUBJECT: Your secret santa recipient is {santee}
    MESSAGE: "\n
      Dear {santa},
      \n\n
      Ho Ho Ho! This year you are {santee}'s Secret Santa!
      \n\n
      Your Secret Santa recipient told Santa that their hobbies include:
        {hobbies}
      \n\n
      Please ship {santee}'s present to the following address by 12/25:
        {address}
      \n\n
      Happy gifting,
      \n
      Santa Claus"

Once configured, call secret-santa:

    python secret_santa.py

Calling secret-santa without arguments will output a test pairing of 
participants.

        Test pairings:

        foo ---> baz
        bar ---> garply
        baz ---> bar
        garply ---> foo

        To send out emails with new pairings,
        call with the --send argument:

            $ python secret_santa.py --send

To send the emails, call using the `--send` argument

    python secret_santa.py --send

Misc.
-----

You can use the following line to get the Google spreadsheet responses
in a copy-pastable format for the template (while disregarding commas):

      awk -vFPAT='([^,]*)|("[^"]+")' -vOFS=, '{print "- "$3"|"$2"|"$4"|"$5}' santas_202X.csv

