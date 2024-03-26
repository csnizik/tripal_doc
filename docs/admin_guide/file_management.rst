File Management
===============

User Quotas
-----------
Data importers that use the Tripal API and Tripal supported widgets automatically associate uploaded files with users. If you are allowing end-users to upload files you may want to consider adding quotas to prevent the server storage from filling.  To ensure that users do not exceed the limits of the server a quota system is available.  Navigate to **Administer > Tripal > Tripal Managed Files** and click the **Disk User Quotas** tab to reveal the following page:

  .. image:: file_management.1.user_quotas.png
        :width: 600
        :alt: Tripal Disk Usage Quotas.

First, the total amount of space consumed by all uploaded files is shown at the top of the page. Initially this will indicate 0 B (for zero bytes); as users upload files this value will change. You may return to this page in the future to check how much space is currently used by user uploads. Here you can also specify the default system-wide quota that all users receive. By default this is set to 64 Megabytes and an expiration of 60 days. Once a file has existed on the site for 60 days the file is marked for deletion and will be removed when the Drupal cron is executed. The default of 64MB per user is most likely too small for your site. Adjust this setting and the days to expire as appropriate for your site's expected number of users and storage limitations and click the **Save** button to preserve any changes you have made.

In addition to the default settings for all users, you may want to allow specific users to have a larger (or perhaps smaller) quota. You can set user-specific quotas by clicking the **Add Custom User Quota** link near the bottom of the page. The following page appears:

  .. image:: file_management.2.user_quotas.png
        :width: 600
        :alt: Tripal Disk Usage Quotas - Custom User Quota.

Here you must specify the Drupal user name of the user who should be granted a custom quota. This field will auto populate suggestions as you type to help you find the correct username. Enter the desired quota size and expiration days and click the **Submit** button. You will then see the user-specific quota listed in the table at the bottom of the page:

  .. image:: file_management.3.user_quotas.png
        :width: 600
        :alt: Tripal Disk Usage Quotas with Custom Quota.

Users' Files
------------
Users with permission to upload files are able to use the Tripal file uploader to add files to the server.  The core Tripal Data Importers use the Tripal file uploader and extension modules may use it as well.  You can enable this functionality for users by Navigating to **Admin** > **People** and click the **Permissions** Tab. next scroll to the **Tripal** section and set the **Upload Files** permissions as desired for your site.  The following screenshot shows the permission on a default Drupal site.

  .. image:: file_management.4.file_upload_permission.png
        :width: 600
        :alt: Tripal Permissions showing File Upload Permissions.

Users who have the ability to upload files can manage files on their own Account pages.

As described in the previous section, the site administrator can set a system-wide or user-specific default expiration number of days for a file. This means files will be removed automatically from the server once their expiration data is set.

.. note::

  Automatic removal of files can only occur if the Drupal cron is setup to run automatically.
