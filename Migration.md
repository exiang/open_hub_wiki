## Init migration
```php 
$migration = Yii::app()->db->createCommand();
```

## Execute raw SQL
```php
$sql = 'UPDATE `eventbrite_organization_webhook` as t LEFT JOIN `organization` as f ON t.organization_code=f.code SET t.organization_id=f.id';
Yii::app()->db->createCommand($sql)->execute();
```

## Create new table
```php
$migration->createTable('eventbrite_organization_webhook', [
    'id' => 'pk',
    'organization_code' => 'varchar(64) NOT NULL ',
    'as_role_code' => "varchar(64) NOT NULL DEFAULT 'owner'",
    'eventbrite_account_id' => 'varchar(64) NOT NULL',
    'eventbrite_oauth_secret' => 'varchar(255) NULL',
    'json_extra' => 'text NULL',
    'is_active' => 'tinyint(1) NOT NULL DEFAULT 1',
    'date_added' => 'integer',
    'date_modified' => 'integer',
]);
$migration->alterColumn('eventbrite_organization_webhook', 'json_extra', 'longtext NULL');
```

## Rename Table
```php
$migration->renameTable('impact2ideabank', 'ideabank_idea2impact');
```

## Insert Data
```php
$migration->insert('ntis_regulatory_body', [
    'code' => 'KPDNHEP',
    'title' => 'Ministry of Domestic Trade and Consumer Affairs',
    'is_active' => 1,
    'date_added' => time(),
    'date_modified' => time(),
]);
```

## Add New Column
with index and foreign key
```php
$migration->addColumn('challenge', 'application_form_id', 'integer NULL AFTER url_application_form');
$migration->createIndex('application_form_id', 'challenge', 'application_form_id', false);
$migration->addForeignKey('fk_challenge-application_form_id', 'challenge', 'application_form_id', 'form', 'id', 'SET NULL', 'CASCADE');
```

## Drop Foreign Key
```php
$migration->dropForeignKey('FK_impact2ideabank_impact_id', 'impact2ideabank');
```

## Drop Table
```php
$migration->dropTable('ideabank_idea2ntis_technology');
```

## Init Meta
```php
MetaStructure::initMeta('organization', 'community', 'urlYoutubeCoverVideo', 'string', 'Cover Youtube Video URL', 'URL to display youtube video in community organization page', '');
```

## Set Service
```php
Service::setService('cv', 'CV Portfolio', 'A talent directory showcasing  experience and qualifications for job opportunity and cofounder matching', array('is_bookmarkable' => 1, 'is_active' => 1));
```

## Set Access Role
```php
Access::setAccessRole('ntis', 'BackendController', ['manageSolutionProvider'], ['ntisTechSecretariat', 'ntisRegulatorySecretariat']);
```

## Set setting
```php
Setting::setSetting('sample-var1', 'Hello World 0.2', 'string');
```

## Set embed
Remember: always prefix your embed with module (e.g. `boilerplateStart-`, `sample-`) or context code (e.g. `cpanel-`)

```php
$embed = Embed::setEmbed('ntis-signup-tncContent', array(
    'is_title_enabled' => true,
    'is_text_description_enabled' => false,
    'is_html_content_enabled' => true,
    'is_image_main_enabled' => false,
    'is_default' => true,
    'title_en' => 'Terms and Conditions',
    'html_content_en' => 'Hello Admin, please remember to update this terms and condition content inside Embed \ <b>#signup-tncContent</b>',
));
```

