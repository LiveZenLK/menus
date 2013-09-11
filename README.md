# CodeIgniter Menus

A flexible, module-ready, low-level solution for menus. Built for CodeIgniter 2.x. This is a low-level, file-based solution that does not provide a GUI of any sort. By using flat files, we can create menus that are simple to maintain in different environments as well as ship with common modules that we create.

It works great with modular solutions out of the box. 

## Initializing the Class

Like most of the standard CodeIgniter libraries, the Menu library  is initialized by using the **$this->load->library** function:

    $this->load->library('menus');

Once the library is loaded it will be ready for use. The menu library object you will use to call all functions is: **$this->menu**.

## Displaying A Menu

Once you have created a json menu file (see below) you can display the menu with the **display()** method.

    <div class="navbar navbar-inverse">
        <div class="navbar-inner">
            <?php echo $this->menus->display('admin'); ?>
        </div>
    </div>

## Menu Files

Each menu is stored as a JSON file under the  _application/menus_ folder. Each menu is maintained in it's own file. The file name should be lowercase and contain no spaces, ending with a _.json_ file extension. For example, the Main Menu for the front of your site might be called simply, **main_menu.json**.

### Menu File Structure

This is an example skeleton menu file.

    {
        "title": "Main Menu",
    	"description": "My menu description.",
    	"restrict_permission": '',
    	"items": [
    		. . .
    	]
    }

**title** Optional. Provided for use by third-party libraries and GUI tools to display information about the menu.

**description** Optional. Provided for use by third-party libraries and GUI tools to display information about the menu.

**restrict_permission** If present, and not empty, this string should be a permission that the user is required to have to view this menu at all. If they do not have the permission the menu will not be displayed. This is currently geared toward use within [Bonfire](http://cibonfire.com), but work is being made to reduce the needs of Bonfire.

**items** An array of menu items to be displayed. The values of each menu item are listed below.

#### Menu Items

    {
    	"title": "Menu Item 1",
    	"slug": "menu_item_1",
    	"route": "/path/to/controller",
    	"weight": 50,
    	"items": [
    		. . .
    	]
    }

**title**: The name, as displayed at the top level of the menu. By default the text will be displayed as shown. If, however, you want the text to be ran through a language file, you can prefix the name with an 'lang:', like "lang:main_menu" and the menu string will be translated into the current language, if the string exists using _lang($name)_ (with the 'lang:' prefix stripped from it).

**slug** A machine-friendly version of the title that module's can reference to insert a menu item under this one. If this is not present, the _title_ field will be used to create one, all lowercase, with spaces replaced by underscores.

**route** The path the user will be sent to when clicking this item. This can be a relative URL or a full URL. Any instances of `{area}` within your route will be replaced with the value of the `SITE_AREA` constant. This is specific to Bonfire, currently, but work is being done to make these more generic and usable across more applications.

**items** An array of additional child menu items under this menu item. 

**weight** A value that defines the relative location within a menu list for that item to appear. The larger the value the more it will "sink" within the list of items.

## Including Other Menus

To help you organize your menus and keep them maintainable, you can easily include another menu.

    {
        "title": "Admin",
        "items": [
            {
                "menu": "developer"
            }
        ]
    }

## Contexts

Contexts provide a simple way to help map your URL to different controllers within your modules. If you have a context named 'settings', then any module controller called 'settings.php' will be mapped to `/settings/{module_name}`. 

The menu system can  automatically scan all of your modules and find the any modules using that context, and builds out a menu for you automatically. 

    {
        "title": "Settings",
        "context": "settings"
    }

## Preferences

The preferences below allow you to tailor the image processing to suit your needs. The should be passed in when the library is initialized.

    $config = array(
        'module_folders'    => array(
            BFPATH  .'modules/',
            APPPATH .'modules'
        ),
        'contexts' => $this->config->item('contexts')
    );

    $this->load->library('menus', $config);

Preference | Default Value | Description
----------------|---------------------|-----------------------------
**menu_path**		| APPPATH/menus	| Where the menu files can be found.
**ttl**							| 300							| The amount of seconds the menu structure should be cached for.
**module_folders**	| array()					| The folders we should look in for modules.
**contexts**					| array()					| The names of valid contexts.
**full_tag_open**		| &lt;ul class="nav">	| The wrapper around the outside of the entire menu.
**full_tag_close**		| &lt;/ul>							| The closing wrapper around the outside of the entire menu.
**tag_open**				| &lt;li>							| Opens each menu item.
**tag_close**				| &lt;/li>							| Closes each menu item.
**dropdown_tag_open**	 | &lt;li class="dropdown">	| The opening tag for a parent item.
**parent_class**			| dropdown-menu	| 
**child_class**				| dropdown				| 
**dropdown_class**	| dropdown-toggle	| 