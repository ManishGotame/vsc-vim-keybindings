# tmux dev session

`start-opp` spins up (or reattaches to) a tmux session named `opp` with all the servers needed for local development, laid out across windows/panes.

## Layout

| Window | Name | Panes |
|---|---|---|
| 0 | `opp-frontend` | `~/workspace/opp/frontend` → `npm run dev` |
| 1 | `opp-backend` | Pane 0: `~/workspace/opp/backend` → Django (`python manage.py runserver`)<br>Pane 1: same dir → Celery worker (`celery -A backend worker --loglevel=info`) |
| 2 | `opp-admin` | `~/workspace/opp/opp-admin-frontend` → `npm run dev` |
| 3 | `paperscrape` | Pane 0: `~/workspace/opp/paperscrape-backend` → Django (`python manage.py runserver`)<br>Pane 1: same dir → Celery worker with queues `default,crawl,parse,classify,publish` (`--pool=solo`) |

If a session named `opp` already exists, the script just attaches to it instead of creating a new one.

## Usage

Make it executable once, then run it whenever you want to start (or resume) development:

```bash
mkdir -p ~/bin
vim ~/bin/start-opp
chmod +x ~/bin/start-opp
echo 'export PATH="$HOME/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
start-opp
```


## Notes

- Assumes the `opp`, `opp-admin-frontend`, and `paperscrape-backend` repos live under `~/workspace/opp/`. Adjust the paths in `start-opp` if your checkout lives elsewhere.
- Backend panes expect a `.venv` virtualenv already created in each backend directory (`python -m venv .venv`).
