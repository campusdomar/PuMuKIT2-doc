PuMuKIT Developer & Architecture Guide
======================================

Architecture
============

Technologies
------------

PuMuKIT has been built using the below technologies. It is recommended to read the documentation of each technology in order to extend PuMuKIT or create new features.

* Symfony 3.4: [web](http://symfony.com) | [doc](http://symfony.com/doc/3.4/index.html)
* PHP7: [web](http://php.net/) | [doc](http://php.net/manual/en/)
* MongoDB 4.0: [web](https://www.mongodb.org/) | [doc](https://docs.mongodb.com/v4.0/)
* Doctrine MongoDB ODM 1.0: [web](http://www.doctrine-project.org/projects/mongodb-odm.html) | [doc](http://doctrine-mongodb-odm.readthedocs.org/en/latest/)
* Bootstrap: [web](http://getbootstrap.com/) | [CSS doc](http://getbootstrap.com/css/) | [Components doc](http://getbootstrap.com/components/) | [JavaScript doc](http://getbootstrap.com/javascript/)
* Material Design [web](http://www.google.com/design/spec/material-design/introduction.html) | [doc](https://fezvrasta.github.io/bootstrap-material-design/)
* FFmpeg: [web](https://www.ffmpeg.org/) | [doc](https://www.ffmpeg.org/documentation.html)
* libav: [web](https://libav.org/) | [doc](https://libav.org/documentation/)


List of Bundles
---------------

PuMuKIT has been developed with the Symfony framework. Symfony is based on bundles, and so, PuMuKIT is structured in bundles.
There is a list of bundles that come activated by default with the project, these are mandatory for the code to work. 
There is also a list of optional bundles that come deactivated. 
In order to install/activate them, follow the instructions in the guide of each bundle.

Activated by default:
* [BaseLivePlayerBundle](https://github.com/pumukit/PuMuKIT/blob/master/src/Pumukit/BaseLivePlayerBundle/Resources/doc/ConfigurationGuide.md): provides a service to show live stream events.
* [BasePlayerBundle](https://github.com/pumukit/PuMuKIT/blob/master/src/Pumukit/BasePlayerBundle/Resources/doc/ConfigurationGuide.md): provides a service to show video.
* [CoreBundle](https://github.com/pumukit/PuMuKIT/blob/master/src/Pumukit/CoreBundle/Resources/doc/ConfigurationGuide.md): provides software functionalities.
* [EncoderBundle](https://github.com/pumukit/PuMuKIT/blob/master/src/Pumukit/EncoderBundle/Resources/doc/ConfigurationGuide.md): provides a service to encode audio/video tracks.
* [InspectionBundle](https://github.com/pumukit/PuMuKIT/blob/master/src/Pumukit/InspectionBundle/Resources/doc/ConfigurationGuide.md): provides a service to inspect multimedia tracks.
* [JWPlayerBundle](https://github.com/pumukit/PuMuKIT/blob/master/src/Pumukit/JWPlayerBundle/Resources/doc/ConfigurationGuide.md): provides the JWPlayer player to show videos.
* [NewAdminBundle](https://github.com/pumukit/PuMuKIT/blob/master/src/Pumukit/NewAdminBundle/Resources/doc/ConfigurationGuide.md): provides the back-office admin UI.
* [SchemaBundle](https://github.com/pumukit/PuMuKIT/blob/master/src/Pumukit/SchemaBundle/Resources/doc/ConfigurationGuide.md): defines the schema of classes and services.
* [StatsBundle](https://github.com/pumukit/PuMuKIT/blob/master/src/Pumukit/StatsBundle/Resources/doc/ConfigurationGuide.md): provides a service to log the statistics.
* [WebTVBundle](https://github.com/pumukit/PuMuKIT/blob/master/src/Pumukit/WebTVBundle/Resources/doc/OverrideGuide.md): defines the portal web.
* [WizardBundle](https://github.com/pumukit/PuMuKIT/blob/master/src/Pumukit/WizardBundle/Resources/doc/ConfigurationGuide.md): provides a service to guide the user on uploading multimedia content.
* [WorkflowBundle](https://github.com/pumukit/PuMuKIT/blob/master/src/Pumukit/WorkflowBundle/Resources/doc/ConfigurationGuide.md): provides a service to automatically extract a picture from a video that just has been transcoded if the MultimediaObject does not have any picture yet.

Not activated by default:
* [NotificationBundle](https://github.com/pumukit/PuMuKIT/blob/master/src/Pumukit/NotificationBundle/Resources/doc/ConfigurationGuide.md): sends emails when a job finished, whether it failed or succeed

List of Events
--------------

Custom events added to PuMuKIT:

* *series.create*: thrown each time a series is created.
* *series.update*: thrown each time a series is updated.
* *series.delete*: thrown each time a series is deleted.

* *multimediaobject.create*: thrown each time a multimedia object is created.
* *multimediaobject.update*: thrown each time a multimedia object is updated.
* *multimediaobject.delete*: thrown each time a multimedia object is deleted.
* *multimediaobject.clone*: thrown each time a multimedia object is cloned.
* *multimediaobject.view*: thrown each time a multimedia object is played in the webtv portal.

* *material.create*: thrown each time a material is added to a multimedia object.
* *material.update*: thrown each time a material in a multimedia object is updated.
* *material.delete*: thrown each time a material is deleted from a multimedia object.

* *track.create*: thrown each time a track is added to a multimedia object.
* *track.update*: thrown each time a track in a multimedia object is updated.
* *track.delete*: thrown each time a track is deleted from a multimedia object.

* *link.create*: thrown each time a link is added to a multimedia object.
* *link.update*: thrown each time a link in a multimedia object is updated.
* *link.delete*: thrown each time a link is deleted from a multimedia object.

* *pic.create*: thrown each time a pic is added to a multimedia object.
* *pic.update*: thrown each time a pic in a multimedia object is updated.
* *pic.delete*: thrown each time a pic is deleted from a multimedia object.

* *personwithrole.create*: thrown each time a person with a given role is added to a multimedia object.
* *personwithrole.update*: thrown each time a person with a given role in a multimedia object is updated.
* *personwithrole.delete*: thrown each time a person with a given role is deleted from a multimedia object.

* *permissionprofile.create*: event is thrown each time a permission profile is created.
* *permissionprofile.update*: thrown each time permission profile is updated.
* *permissionprofile.delete*: thrown each time permission profile is deleted.

* *user.create*: thrown each time a user is updated.
* *user.update*: thrown each time a user is updated.
* *user.delete*: thrown each time a user is deleted.

* *group.create*: thrown each time a group is updated.
* *group.update*: thrown each time a group is updated.
* *group.delete*: thrown each time a group is deleted.

* *job.success*: thrown each time a job is finished successfully in the system.
* *job.error*: thrown each time a job fails in the system.

* *wizard.form.submit*: thrown each time a wizard form is submitted.

* *annotations.update*: thrown each time an annotation is edited (through put)

To add more events, read the Symfony documentation about creating [custom events](http://symfony.com/doc/current/components/event_dispatcher/introduction.html#creating-and-dispatching-an-event) and [listeners](http://symfony.com/doc/current/cookbook/event_dispatcher/event_listener.html).


How to extend Pumukit
=====================

Best practices:
* Do not modify PuMuKIT Bundles, extend them following the Symfony documentation about [overriding Bundles](http://symfony.com/doc/current/cookbook/bundles/inheritance.html).
* Create your own Bundles and add them to the PuMuKIT project.
* All the bundles created or overridden should be inside an organization directory:

```
src/Pumukit/ExampleOrg/Feature1Bundle
src/Pumukit/ExampleOrg/Feature2Bundle
src/Pumukit/ExampleOrg/Feature3Bundle
...
```

Override a Bundle
-----------------

See an example in [WebTVBundle](https://github.com/pumukit/PuMuKIT/blob/master/src/Pumukit/WebTVBundle/Resources/doc/OverrideGuide.md).


Create a new Bundle
----------------------

#### 1.1 Generate the bundle.

```
$ php app/console generate:bundle --namespace=Pumukit/ExampleOrg/FeatureBundle --dir=src --no-interaction
```

#### 1.2 Activate the new bundle.

Add to app/config/AppKernel.php the namespace of your new bundle.

Show [Symfony Bundle System](https://symfony.com/doc/3.4/bundles.html) to get an example

If your bundle have route annotations or route file you must add to app/config/routing.yml

Show [Symfony Routing](https://symfony.com/doc/3.4/routing.html) to get an example


#### 1.3 Develop the bundle

Create all Documents, [Services](http://symfony.com/doc/current/book/service_container.html), Events, Event Listeners, [Controllers](http://symfony.com/doc/current/book/controller.html), [Commands](http://symfony.com/doc/current/bundles/SensioGeneratorBundle/commands/generate_command.html) and HTML Twig templates as needed, following the bundles structured defined by Symfony.

Main URLs of a PuMuKIT deployment
---------------------------------
* Web Portal: `http://{MyPuMuKIT_IP}/`
* Back-office (admin interface): `http://{MyPuMuKIT_IP}/admin`
