# AUTOLab Machine Stats

This repo is forked from [**Hivemind**](https://github.com/compserv/hivemind) for
Berkeley EECS instructional computers.

## How does it work?

Every five minutes, `backend/census.py` is executed. It connects to each server
listed in `backend/server.txt` via SSH and collects information. The results
from all of the servers are combined into a single JSON file
(`data/latest.json`).

### Overall load formula

The "overall load" heuristic is implemented in `toRating()` in
[`main.js`][main-js].

[main-js]: https://github.com/compserv/hivemind/blob/master/js/main.js

## Contributing

If you would like to add / remove servers from the list, please file
an issue or a pull request with your requested changes.

The current list of servers is at [backend/servers.txt][servers.txt].

## Development

Want to host the website locally? Clone this repo, and start a web server in
the project root directory.

The backend (i.e. the script that grabs data from the servers) is a little
harder to set up:

0. Clone this repo.
1. Run `make venv` to create a virtualenv and install
   [paramiko](https://pypi.python.org/pypi/paramiko). You can also run `pip install
   paramiko`, but you'll probably want to install it locally to avoid polluting
   your system libraries, so a virtualenv works well for that.
2. Create an RSA key pair with no passphrase, rename the private key to
   `hivemind_rsa` and the public key to `hivemind_rsa.pub` and put them inside
   your home directory's SSH directory (`~/.ssh`).
3. Add the public key to your class account's `~/.ssh/authorized_keys` file to
   allow hivemind to log in to the servers automatically.
4. Change the value of `LOGIN_USERNAME` in `census.py` to your login.

You should then be able to execute `census.py` to grab data from each server in
`servers.txt`. The results are printed to stdout, which `run_census` puts into
a file for the frontend to fetch.