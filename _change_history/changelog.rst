############
Changelog
############

|

.. code-block:: shell

   Titled as Changelog (not Change Log)

   One Changelog (*.rST file) for each major version. No separate Changelogs for minor versions or patch versions.

   Within the Changelog, there is one section per patch version/minor version.

   Each section starts with a horizontal line, followed by the section title and optional section subtitle.

   For REST APIs the Section Title is the release date. The release number is optional (and often irrelevant)

   Within each section, list the changes under the following headings

      Added - for new features.

      Changed - for changes to existing functionality.

      Deprecated - for soon to be removed features.

      Retired - for features that have been removed.

      Fixed - for any defect fixes.

      Security - for vulnerability related changes.

      As far as possible, each change should read together with its heading as though it is a single sentence.


    Target Audience:

      - An App developer who has already used the API.
      - A Support personal checking to see if the dates of any recent changes coincide with users reporting issues

    Purpose: To make it easier to discover noteworthy changes in a given version.

    Answers the following questions:

        What has changed since I last updated my App?

        Has the bug that blocked me been addressed?

        Is the workaround I put in place still required?

        Is there anything new that could add value to my App?

        Will any changes break my current implementation?

        My App that was working perfectly has started crashing.
        Was a recent update the cause of my problem?
        If so, what are the changes that could likely affect me?

|

=======================================================================================================================

************************
Release Date: 2018-03-26
************************

*Version 2.0.17*

Added
=====

- Restore support for BIM 360 Docs folder in `PATCH projects/:project_id/folders/:folder_id </en/docs/data/v2/reference/http/projects-project_id-folders-folder_id-PATCH/>`_ endpoint.
- Create item support for BIM 360 Docs in `POST projects/:project_id/items </en/docs/data/v2/reference/http/projects-project_id-items-POST/>`_ endpoint.
- Create version support for BIM 360 Docs in `POST projects/:project_id/versions </en/docs/data/v2/reference/http/projects-project_id-versions-POST/>`_ endpoint.

|

=======================================================================================================================

************************
Release Date: 2018-03-08
************************

*Version 2.0.16*

Fixed
=====

- Issue with blah blah blah.

|

=======================================================================================================================

************************
Release Date: 2018-03-01
************************

*Version 2.0.15*

Added
=====

- Ability to create folders for BIM 360 Docs in `POST projects/:project_id/folders </en/docs/data/v2/reference/http/projects-project_id-folders-POST.rst/>`_ endpoint.
- Copy version support for BIM 360 Docs in `POST projects/:project_id/versions </en/docs/data/v2/reference/http/projects-project_id-versions-POST.rst/>`_ endpoint.

|

=======================================================================================================================

************************
Release Date: 2018-02-09
************************

*Version 2.0.14*

Added
=====

- `reserved`, `reservedTime`, `reservedUserId`, `reservedUserName` attributes to all endpoints that returns an `Item entity </en/docs/data/v2/overview/field-guide/#item>`_

Fixed
=====

- A360 Preview tab images are now loaded.
- Issue with confusing icon state, when pressing the ESC key in the LMV window.

|

=======================================================================================================================

************************
Release Date: 2018-01-29
************************

*Version 2.0.13*


Added
=====

- Copy to folder support for BIM 360 Docs in `POST projects/:project_id/items </en/docs/data/v2/reference/http/projects-project_id-items-POST/>`_ endpoint.

Fixed
=====

- Issue with `lastModifiedTimeRollup` attribute in `Folder entity </en/docs/data/v2/overview/field-guide/#folder>`_ not comp;lying with the ISO8601 format.

Removed
=======

- Empty sections from CHANGELOG, which occupied too much space.

|

=======================================================================================================================

************************
Release Date: 2018-08-10
************************

*Version 2.0.1*

Changed
=======

- | The hubs object response now distinguishes between team hubs and personal hubs. Team hubs are represented by ``hubs:autodesk.core:Hub`` (which previously represented all hubs) and personal hubs are represented by ``hubs:autodesk.a360:PersonalHub`` (a new type).

  |**Note** The previous representation for all hubs (``hubs:autodesk.core:Hub``) now only represents team hubs,  any code that previously, explicitly checked for hubs will now only return team hubs, unless you update the code.

  | See `GET hubs </en/docs/data/v2/reference/http/hubs-GET/>`_ for more information.

Removed
=======

- Support for uploading of Fusion 360 Designs.

|


=======================================================================================================================

************************
Release Date: 2016-08-26
************************

*Version 2.0.0*

Added
=====

- Support for creating folders using `POST projects/:project_id/folders </en/docs/data/v2/reference/http/projects-project_id-folders-POST/>`_
- Ability to modify metadata and core attributes of folders, items, and versions via PATCH.
- Ability to create ownloads of Fusion 360 Designs such as .f3z/.f3d files with `POST projects/:project_id/downloads </en/docs/data/v2/reference/http/projects-project_id-downloads-POST/>`_

Changed
=======

- Creating folders, items, versions, and relationships now require an extension section that specifies the type and version of the respective type to be created.
- The payload passed with a POST request to create folders, items, version, and relationships is now fully validated against the referenced schema.
- Folders, items, and versions now expose an additional ``/links`` relationship.
- Relationship links to thumbnails and manifests now point to the respective Model Derivative API endpoints.

Deprecated
==========

- Support for uploading of Fusion 360 Designs.
