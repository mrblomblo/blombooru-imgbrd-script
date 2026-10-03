# Blombooru imgbrd-grabber script

> [!IMPORTANT]
> **This repo is archived and obsolete as it is being officially added to Grabber in https://github.com/Bionus/imgbrd-grabber/pull/3728!**

## 1. Install Blombooru

Follow the official [Quick Start](https://github.com/mrblomblo/blombooru#quick-start-pre-built-image) documentation from the Blombooru repository.
Note that you'll need to have [Docker](https://docs.docker.com/get-docker/) installed.

It pretty much only amounts to downloading the `docker-compose.yml` and `example.env` files, renaming the latter to `.env`, and doing `docker compose up -d`. Blombooru defaults to port `8000`.


### Configuration

- Open your instance at `http://localhost:8000/` and go through the onboarding to set your admin username and password
- Open the admin panel, go to the "Account" section and create a new API key. Make sure to set its permission level to at least "Write" (an Admin-level key also works, but is absolutely not recommended), otherwise uploads will be rejected with a 403 error
- Copy the generated key (it starts with `blom_` and is only ever shown once)

## 2. Grabber

### Install NodeJS

You need Node.js to be installed on your machine to use the upload script used by Grabber.  
You can download it from [their website](https://nodejs.org/en/download/), or from a package manager [here](https://nodejs.org/en/download/package-manager/).


### Download the upload script

Download the [blombooru.js](blombooru.js) and JSON files into a "blombooru" folder in Grabber's installation folder. You *can* place it wherever, as long as you copy the exact file location and replace the `blombooru/blombooru.js` part in the command later shown.

> [!NOTE]
> If your Blombooru instance is not on the same machine as Grabber, or simply not accessible at `http://localhost:8000/`, make sure to update `BASE_URL` in the script.


### Install NodeJS packages

This script uses the Node.js [axios](https://www.npmjs.com/package/axios) and [form-data](https://www.npmjs.com/package/form-data) packages. You can install them by running the following command in the same directory as the script and JSON files:

```bash
npm install
```

### Configuration

Open Grabber, click "Tools" then "Options", then when the options window has opened, go to "Commands" and set the "Image" field to:

```bash
node blombooru/blombooru.js "YOUR_API_KEY" "%path:nobackslash%" "%all:includenamespace,unsafe,underscores%" "%rating%" "%source:raw%"
```

Make sure to replace `YOUR_API_KEY` with the API key you created earlier (including the `blom_` prefix).  
On Windows, you may also need to replace `blombooru/blombooru.js` with `blombooru\blombooru.js`, but I don't use it so I can't verify if that is necessary.

This command will be run every time an image is saved, causing it to also be sent to your Blombooru instance!

> [!NOTE]
> Blombooru only has three ratings (`safe`, `questionable`, `explicit`) and five tag categories (`general`, `artist`, `character`, `copyright`, `meta`). The script automatically maps whatever Grabber provides onto these, collapsing anything in between (e.g. "sensitive"/"sketchy") into `questionable`, and anything outside those five namespaces into `general`.
