# Theater Inventory

A simple inventory tracker for a school theater. Students sign in with an account you create, tap + or − to add or take items, and everyone sees who last changed each item. A Google Sheet is the database, so the data is always there to view, sort or export.

**How it fits together**

- `index.html` is the whole app. Host it free with GitHub Pages.
- `apps-script/Code.gs` runs inside your Google Sheet. It handles sign-in and saves every change with the student's name.
- `.github/workflows/backup.yml` is the GitHub agent. Each morning it copies the inventory from the Sheet into `data/` in this repo.

## Setup (about 15 minutes)

### 1. Create the Google Sheet backend

1. Make a new Google Sheet and name it "Theater Inventory".
2. Open **Extensions > Apps Script** and replace the default code with the contents of `apps-script/Code.gs`.
3. Near the top, set `ADMIN_USERNAME`, `ADMIN_NAME` and `ADMIN_PIN` for your own admin account.
4. Choose `setup` in the function dropdown and select **Run**. Approve the permissions when asked. This creates the Inventory, Log and Users tabs.
5. Delete your PIN from the `ADMIN_PIN` line and save. It is only needed once.

### 2. Publish the backend

1. In Apps Script, select **Deploy > New deployment > Web app**.
2. Set **Execute as: Me** and **Who has access: Anyone**. (People still need a username and PIN to see or change anything.)
3. Select **Deploy** and copy the **Web app URL**.
4. Open `index.html`, find `API_URL`, and paste the URL between the quotes.

If you edit `Code.gs` later, use **Deploy > Manage deployments > Edit > New version** so the change goes live.

### 3. Put the site on GitHub Pages

1. Create a GitHub repo and upload all of these files, keeping the folders.
2. In the repo, go to **Settings > Pages**, choose **Deploy from a branch**, pick `main` and `/ (root)`, and save.
3. After a minute your site is live at `https://YOUR-NAME.github.io/REPO-NAME/`.

### 4. Create student accounts

Sign in as admin and open the **Students** tab. Enter a name (this is what appears in the log), a username and a 4 to 8 digit PIN. Give those to the student. You can reset a PIN or remove an account at any time, and removed accounts are signed out right away.

### 5. Turn on the nightly backup (optional)

1. In the repo, go to **Settings > Secrets and variables > Actions** and add two secrets:
   - `APPS_SCRIPT_URL`: the Web app URL from step 2.
   - `BACKUP_KEY`: in Apps Script, open **Project Settings > Script properties** and copy the `BACKUP_KEY` value.
2. Run it once from the **Actions** tab to test it. You should see `data/inventory.csv` appear.

## NFC tags

Open any item and select **Copy tag link**. Write that link to an NFC tag (any free NFC writer app works). Tapping the tag opens the site and highlights that item, ready for + or −. Students sign in once and their phone remembers them.

## Good to know

- **Who changed what:** each item shows who last changed it, and the **Activity** tab lists every add, take, edit and delete with names and times. The same history lives in the Log tab of your Sheet.
- **Do not share the Google Sheet with students.** The Users tab holds PIN hashes, and sharing would let people edit the data directly. They only need the website.
- **Student privacy:** the backup in `data/` leaves out who changed things, so a public repo does not expose student names. Names stay in your Sheet.
- **Security level:** short PINs are fine for keeping a school theater tidy and accountable. They are not meant for anything sensitive. Five wrong tries locks that username for 5 minutes.
- **Everyone can delete items,** but deletes are logged with the student's name.