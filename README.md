
# Scrappr Central Repository

This is the central hub for the Scrappr app family, containing:

- **Original web app**
- **Browser extension**
- **Companion mobile app**

## Cloning and Fetching Submodules

After cloning this repository, submodule folders will be empty. To fetch the contents for each project, run the following commands from the root directory:

- **Web app:**
	```bash
	git submodule update --init scrappr
	```

- **Browser extension:**
	```bash
	git submodule update --init scrappr-extension
	```

- **Mobile app:**
	```bash
	git submodule update --init scrappr-mobile
	```

## Keeping Submodules Up to Date

Whenever you commit changes to a submodule and want the central repository to track the latest versions, run:

```bash
git submodule update --init --recursive --remote
```

This will update all submodules to their latest commits.