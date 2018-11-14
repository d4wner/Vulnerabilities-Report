Well ,sir ,I just found a Any-Directory-Traversal bug here.

#### ADLab of Venustech

The report link to the wordpress-form is missing, because the manager do not wish to put the public in danger ,I'll just write some details here.

When I login into the wordpress panel, assume I have a low privilege role like a editor user.

Because the admin user has turned on the option of the wp-plugin Media from FTP, a normal user like me can also use it.

When I visit the page as a normal user:

```
http://localhost/wordpress/wp-admin/admin.php?page=mediafromftp-search-register
```

It's should be a function of the plugin ,which can view some normal files at the website folder.

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/media-from-ftp/search.png)

Then I click the search button in the page, post something to the url:

Weak post para:

```
searchdir=wp-content/../../
```

We can change the value of the para "searchdir", and we'll view any other files or folders at the server, not only the website.

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/media-from-ftp/dir.png)
