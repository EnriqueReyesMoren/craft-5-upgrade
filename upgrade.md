# Black Magic
## _From Craft 4x to Craft 5 BM Guide_


This step by step guide covers the process to update a Craft 4 instance to a Craft 5x instance. 
Additionally it cover some plugins updates in order to keep all the enviroment up to date. 

## Manual/Action required migration Plugins

- Freeform
- Typelink to Hyper
- Redactor to CK Editor ✨

## Before start the upgrade

- Backup Current Database
- Backup Current Files 
- Check enviroment requeriments and update it

## Let’s take a moment to audit and prepare your project.

Your live site must be running the latest version(opens new window) of Craft 4;
- The most recent Craft 4-compatible versions of all plugins are installed, and Craft 5-compatible versions are available;
- Your project is free of deprecation warnings after thorough testing on the latest version of Craft 4;
- All your environments meet Craft 5’s minimum requirements (the latest version of Craft 4 will run in any environment that meets Craft 5’s requirements, so it’s safe to update PHP and your database ahead of the 5.x upgrade):
-- PHP 8.2
-- MySQL 8.0.17+ using InnoDB, MariaDB 10.4.6+, or PostgreSQL 13+
- You’ve reviewed the breaking changes in Craft 5 further down this page and understand that additional work and testing may lie ahead, post-upgrade;

Once you’ve completed everything above, you’re ready to start the upgrade process!

## Upgrading the plugins before Craft 5X upgrade
**REDACTOR TO CK EDITOR**

- Install CK Editor;
- In CK Editor Settings replicate the current redactor config file via Drag en drop setup builder;
- After you already create the config setup on the CK Editor settings, go to Settings > Fields in the Craft cms dashboard
- Change the field type of all the redactor fields and make sure the previusly created config file is the default selected 
- 


**Typelink to Hyper**

- Install and activate Hyper;
- Go to Settings > Plugins > Hyper;
- Select under the Migrations tab the option "Typelink field"
- Click the first Step Action: Migrated Fields (For this action you need to have the parameter "Allowadminchanges" set on true or make this change on production/staging site.  
- Go to Step 2: Migrate Content and click the button to start the process
- After that actions made, all the fields should be replaced and now pointing to Hyper type. 
- Go to all the templates that use typelink fields, and change the markup for something like this: 
- 

**Freeform 4 to 5**

- Carefully review the changelog for Freeform 5.x as well as the new key features/changes table above.
- Edit your project's composer.json file and update the Freeform version to 5.x:
```sh
  "require": {
      "craftcms/cms": "^4.0.0",
      "vlucas/phpdotenv": "^5.4.0",
-     "solspace/craft-freeform": "^4.0.0",

+     "solspace/craft-freeform": "^5.0.0",

      "solspace/craft-calendar": "^4.0.0"
  },
```
- Edit your project's composer.json file and update the Freeform version to 5.x:
- Get the latest 5.x package of Freeform by running the following command:
```sh
  php craft update freeform
```
- In your CLI, run the following command:
```sh
  php craft migrate --plugin=freeform
```
- In your CLI, run the following command:
```sh
  [username@Computer site.test % php craft migrate --plugin=freeform 
Checking for pending Freeform migrations ...
Total 29 new migrations to be applied:
    m230101_100000_ConvertJsonToLongTextColumns
    m230101_100010_FF4to5_MigrateForms
    m230101_100020_FF4to5_MigrateLayout
    m230101_100030_FF4to5_MigrateNotifications
    m230101_100050_FF4to5_MigrateIntegrations
    m230101_100060_FF4to5_MigrateConditionalRules
    m230101_200000_FF4to5_MigrateData
    m230101_300100_FF4RemoveOldTables
    m230224_141036_RemoveRedundantFieldsFromIntegrationsTable
    m230224_141037_RenameIntegrationTableColumns
    m230227_102619_MoveCRMIntegrationClasses
    m230301_124411_MoveMailingListIntegrationClasses
    m230712_120518_RemoveIntegrationsQueueMailingListFieldIndex
    m230725_124256_AddCategoryToCrmFields
    m230809_081815_AddCategoryToMailingListFields
    m230824_111101_ChangeMailingListsToEmailMarketing
    m230824_163145_RemoveWebhooksTable
    m230920_103014_RemoveLastUpdateFromIntegrations
    m230925_162351_AddEnabledToIntegrations
    m231020_115409_MigrateIntegrationNamespaces
    m231116_104621_AlterPaymentTables
    m231128_142144_AddLinkToPaymentsTable
    m231206_132139_RemoveLockTable
    m231219_105754_AddUsersToFormsTable
    m231229_155623_CreateSurveysPreferencesTable
    m231230_074448_CreateFieldsTypeGroupsTable
    m240109_142124_UpdatePageButtonMetadata
    m240110_111258_ChangeFormFieldRowForeignKey
    m240111_162954_RemoveStatisticsWidgetFromWidgetsTable

Apply the above migrations? (yes|no) [no]:yes
```
-- There will almost certainly be some amount of cleanup required for every site.

- Be sure to apply any changes noted in the key features/changes table above.
- Carefully review and update all Freeform settings, submission data and forms in the form builder.
- Also be sure to review and update your templates and formatting templates (if you have any custom ones) and ensure that everything works correctly and any necessary updates are made.
- If you plan to use the demo templates to review/text/experiment, please reinstall them if you already installed then in an earlier version of Freeform.

**Freeform migration fails scenarios**

You can address some issues running the updates due a SQL migration fail proccess and the CMS is going to show some error windows like the one showing below 

https://files.slack.com/files-pri/T06C2G8K0-F0728BA6DUJ/image.png
https://files.slack.com/files-pri/T06C2G8K0-F072AR6SLGL/image.png
https://files.slack.com/files-pri/T06C2G8K0-F072ARG2AKE/image.png

If this particular scenario the plugins needs to be totally unistall and removed from the project. 
Before that you need to back up the submission with a CSV, XLS export inside the Freeform dashboard in the CMS 

https://files.slack.com/files-pri/T06C2G8K0-F072H9ZSF60/image.png

After the previous step you need to run the following commands in the terminal 

```sh
php craft plugin/disable freeform
php craft plugin/uninstall freeform
```

Then you need to remove this line in the composer.json
```sh
php craft plugin/disable freeform
php craft plugin/uninstall freeform
```

Then remove/drop this tables from the database
https://files.slack.com/files-pri/T06C2G8K0-F072ATTT1MJ/image.png

Finally you need to reinstall the plugin and create manually the forms. 

## Performing the upgrade for plugins left before migration:
- Upgrade all the plugins in order to match the requeriments in the dashboard on the section "Upgrade to Craft 5" running the following command: 
```sh
php craft update all
```
- In case that some plugins don´t upgrade you need to update one by one in the CMS or running the php craft update plugin-handle 
- You’ll need to manually edit each plugin version in composer.json. If any plugins are still in beta, you may need to change your minimum-stability(opens new window) and prefer-stable(opens new window) settings.

Ready to go 


## Performing the Upgrade 

-- Capture a fresh database backup from your live environment and import it.
-- Make sure you don’t have any pending or active jobs in your queue.
-- Run php craft project-config/rebuild and allow any new background tasks to complete.
-- Capture a database backup of your local environment, just in case things go sideways.
-- Note your current Temp Uploads Location setting in Settings → Assets → Settings.
-- Add your database’s current character set and collation to .env. If you have always used Craft’s defaults, this will be:
```sh
CRAFT_DB_CHARSET="utf8mb3"
CRAFT_DB_COLLATION="utf8mb3_general_ci"
```
-- Edit your project’s composer.json to require "craftcms/cms": "^5.0.0" and Craft-5-compatible plugins, all at once.
```sh
"require": {
        "craftcms/cms": "5.1.1",
    },
 ```

While you’re at it, review your project’s other dependencies! You may also need to add "php": "8.2" to your platform(opens new window) requirements
```sh
    "config": {
        "platform": {
            "php": "8.2"
        }
    },
 ```
 
-- Run composer update.
```sh
   composer update
 ```

-- Make any required changes to your configuration.

-- Run php craft up.
```sh
   php craft up.
 ```
 
 https://files.slack.com/files-pri/T06C2G8K0-F071QHJBPTM/image.png

-- Remove your database character set and collation settings from .env (and db.php—even if you didn’t modify it during the upgrade), then run php craft db/convert-charset.
```sh
   php craft db/convert-charset
 ```
 
https://files.slack.com/files-pri/T06C2G8K0-F071A4WKFE3/image.png

** Your site is now running Craft 5! If you began this process with no deprecation warnings, you’re nearly done. **

** Once you’ve verified everything’s in order, commit your updated composer.json, composer.lock, and config/project/ directory (along with any template, configuration, or module files that required updates) and deploy those changes normally in each additional environment.**


## Config and notes

Database Character Set and Collation
MySQL 8.0 deprecated the utf8mb3 character set(opens new window), as well as the use of utf8 as an alias for utf8mb3(opens new window). MySQL recommends utf8mb4(opens new window) instead, and it’s expected that utf8 will become an alias for utf8mb4 in MySQL 8.1.

To ease that transition, Craft 5 is proactively treating utf8 as utf8mb4 for MySQL and MariaDB installs. Since that will be different from what most Craft installs are already using, you will need to start explicitly setting the charset and collation using the non-aliased names, to avoid a SQL error during the upgrade.



## License


**Making the internet a happy place**

