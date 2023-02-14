Apologies for adding all of my code here. I was having difficulties pushing it to github. I will have it fixed for the next lab. Thank you
# Lab 3
[Fork](https://docs.github.com/en/get-started/quickstart/fork-a-repo) this repo and clone it to your machine to get started!

## Team Members
- team member 1 Brendan Best
- team member 2

## Lab Question Answers

Answer for Question 1: 
Statelessness removes server load because the server does not have to retain past client request information. Well-managed caching partially or completely eliminates some client-server interactions.

Question 2:
the resources the mail server is providing to the user is differnt text files. 

Question 3: 
The layered system is not used in this mail server. We could create another server with a differnt functionality and use it to speed up and aid our mail server. 
Question 4:
Many RESTful APIs, including public APIs, require a key. The reasons an API provider may require a key include the following:to authenticate the source to make sure it is authorized to use the API, to limit the number of people using the API, to better capture and track the data being requested by users
to gather information on the people using the API.

https://itexamanswers.net/question/what-are-two-reasons-that-most-restful-apis-require-a-key-in-the-request-choose-two#:~:text=Explanation%3A%20Many%20RESTful%20APIs%2C%20including%20public%20APIs%2C%20require,sure%20it%20is%20authorized%20to%20use%20the%20API

Mail client
def get_inbox(recipient: str) -> None:
    """TODO: fill out this docstring (using the send_mail docstring as a guide)
	it gathes all mail recieved by the mail server
	returns a list of all the emails receieved
    """
    response = requests.get(f'{SERVER}/mail/inbox/{recipient}')
    pprint.pprint(response.json())

def get_sent(sender: str) -> None:
    """TODO: fill out this docstring (using the send_mail docstring as a guide)
   gathers all the emails sent through the client
	returns a list of all the emails sent
    """
    response = requests.get(f'{SERVER}/mail/sent/{sender}')
    pprint.pprint(response.json())

def get_mail(mail_id: str) -> None:
    """TODO: fill out this docstring (using the send_mail docstring as a guide)
	gathers all the mail id's that hav epassed throught eh server
returns a list of the mail id's
    """
    response = requests.get(f'{SERVER}/mail/{mail_id}')
    pprint.pprint(response.json())

def delete_mail(mail_id: str) -> None:
    """TODO: fill out this docstring (using the send_mail docstring as a guide)
	finds the entered mail id and then deletes its 
	returns a list of remaining emails
    """
    response = requests.delete(f'{SERVER}/mail/{mail_id}')
    pprint.pprint(response.json())


Mail Server
def save_mail(mail: List[Dict[str, str]]) -> None:
    """TODO: fill out this docstring (using the load_mail docstring as a guide)
	takes the file that was loaded from the json file and saves it 
    """
    thisdir.joinpath('mail_db.json').write_text(json.dumps(mail, indent=4))

def add_mail(mail_entry: Dict[str, str]) -> str:
    """TODO: fill out this docstring (using the load_mail docstring as a guide)	
	takes loaded mail file and adds a id number to it. returns id number
    """
    mail = load_mail()
    mail.append(mail_entry)
    mail_entry['id'] = str(uuid.uuid4()) # generate a unique id for the mail entry
    save_mail(mail)
    return mail_entry['id']

def delete_mail(mail_id: str) -> bool:
    """TODO: fill out this docstring (using the load_mail docstring as a guide)
	Takes selected mail file and checks it id number with that of the user input and verifies thats its the correct file and then deletes it.
    """
    mail = load_mail()
    for i, entry in enumerate(mail):
        if entry['id'] == mail_id:
            mail.pop(i)
            save_mail(mail)
            return True

    return False

def get_mail(mail_id: str) -> Optional[Dict[str, str]]:
    """TODO: fill out this docstring (using the load_mail docstring as a guide)
	it runs a loop to find the mail with the matching id number and then returns the given mail file
    """
    mail = load_mail()
    for entry in mail:
        if entry['id'] == mail_id:
            return entry

    return None

def get_inbox(recipient: str) -> List[Dict[str, str]]:
    """TODO: fill out this docstring (using the load_mail docstring as a guide)
	Takes saved mail files and adds them to a list. returns list of all mail files 
    """
    mail = load_mail()
    inbox = []
    for entry in mail:
        if entry['recipient'] == recipient:
            inbox.append(entry)

    return inbox

def get_sent(sender: str) -> List[Dict[str, str]]:
    """TODO: fill out this docstring (using the load_mail docstring as a guide)
	takes selected mail file and sends it to specified location 
    """
    mail = load_mail()
    sent = []
    for entry in mail:
        if entry['sender'] == sender:
            sent.append(entry)

    return sent

# API routes - these are the endpoints that the client can use to interact with the server
@app.route('/mail', methods=['POST'])
def add_mail_route():
    """
    Summary: Adds a new mail entry to the json file

    Returns:
        str: The id of the new mail entry
    """
    mail_entry = request.get_json()
    mail_id = add_mail(mail_entry)
    res = jsonify({'id': mail_id})
    res.status_code = 201 # Status code for "created"
    return res

@app.route('/mail/<mail_id>', methods=['DELETE'])
def delete_mail_route(mail_id: str):
    """
    Summary: Deletes a mail entry from the json file

    Args:
        mail_id (str): The id of the mail entry to delete

    Returns:
        bool: True if the mail was deleted, False otherwise
    """
    # TODO: implement this function
    delete = jsonify({'deleted':delete_mail(mail_id)})
    delete.status_code =200
    return delete

Weather

URL = "https://api.openweathermap.org/data/2.5/weather"

# TODO: get an API key from openweathermap.org and fill it in here!
API_KEY = "953f1c228eba49d5fdd4c464288ec08"

def get_weather(city) -> Dict:
    res = requests.get(URL, params={"q": city, "appid": API_KEY})
    return res.json()





...
