Yii2 Template Module
==============

Module Template for quickly write composer module. Just extract it and search replace the work `template` with `yourModuleName`. Then rename the file `TemplateModule` to `YourModule`. 

That is all!

## Install

Modify the composer.json of your project:

~~~
[json]
 "repositories": [
        ...
        {
          "type": "vcs",
          "url": "https://github.com/istt/yii2-template-module",
          "reference":"master"
        },
        ...
],
"require": {
                ...
                "istt/yii2-template-module":"*",
                ...
        },
~~~

Then run the following commands:

~~~
[bash]
php composer.phar update
./yii migrate/up --migrationPath=@vendor/istt/yii2-template-module/migrations
~~~

Last, add the module to your config file

~~~
[php]
	'modules' => [
		...
		'project' => 'istt\template\TemplateModule',
		...
	],
~~~

In your main layout file:

~~~
[php]
$items =  [
        ['label' => 'Home', 'url' => ['/site/index']],
        ['label' => 'About', 'url' => ['/site/about']],
        ['label' => 'Contact', 'url' => ['/site/contact']],
        Yii::$app->user->isGuest ?
            ['label' => 'Login', 'url' => ['/site/login']] :
            ['label' => 'Logout (' . Yii::$app->user->identity->username . ')',
                'url' => ['/site/logout'],
                'linkOptions' => ['data-method' => 'post']],
    ];
 foreach (\Yii::$app->modules as $id => $child) {
	$module = \Yii::$app->getModule($id);
	if ($module && (file_exists($phpFile = $module->getViewPath() . '/layouts/_menu' . ucfirst($id) . '.php'))) {
		$items = array_merge_recursive($items, require($phpFile));
	}
}
~~~