# Core Concepts in the Team-One Apis

A brief overview of the primary objects or "resources" within&nbsp;Team One.
This document is *not* a data-dictionary nor a comprehensive technical reference.
It *is* an introduction to some of the core concepts you'll encounter within the Team One API, intended to help you better understand and navigate the Team One framework.

<p><i style="color:#666">Relationships between several core entities within Team One.</i></p>
</div>

![](https://raw.githubusercontent.com/BroadSoft-Xtended/DeveloperPortalDocs/master/TeamOne/images/coreConcepts1.png)

## Users

A ***user*** is an account within Team One.

*Users* have a handful of common attributes, including:

 * `user_id` - an arbitrary, unique identifier for the *user* (assigned).
 * `given_name` - first name (required).
 * `family_name` - last name (required).
 * `email` - email address (required).
 * `title` - job title (optional).
 * `tel_work` - work phone (optional).
 * `tel_cell` - mobile phone (optional).
 * `avatar` - URL of the user's avatar image.

Team One *user* attributes are inspired by and largely compatible
with the [vCard](http://en.wikipedia.org/wiki/VCard) standard.



## Notes

A ***note*** is the fundamental unit of content within Team One.

*Notes* have several core attributes, including:

 * `note_id` - an arbitrary, unique identifier for the *note* (assigned).
 * `title` - a label or subject for the *note* (optional).
 * `body` - the text of the *note* (optional).
 * `note_type` - the kind or sub-type of note, such as "task" (required).

Team One defines several kinds of *note*, such as NOTEs, TASKs, CHATs, RESOURCEs (files) and REPLYs (comments). These are distinguished by the `note_type` attribute.

Different types of *note* may include additional attributes.  For example, a *task* is a *note* that includes attributes for tracking to whom a task is assigned and when the task is due.

*Notes* often have:

 - *attachments* - documents or files associated with the *note*
 - *replies* - a comment attached to the *note*, and
 - *tags* - free-text labels, keywords or categories used to organize *notes* in a type of [folksonomy](http://en.wikipedia.org/wiki/Folksonomy).

(The Team One API and this documentation sometimes use the term "note" to refer to all *note* types generically.)

## Workspaces

A ***workspace*** provides a place for one or more *users* to create, share and collaborate around *notes* (of all types).

Each *workspace* defines a small number of core attributes, including:

 * `workspace_id` - an arbitrary, unique identifier for the *workspace* (assigned).
 * `name` - a label or title for the *workspace* (required).
 * `description` - a brief summary of the goal or intent behind the *workspace* (optional).

Every *note* belongs to exactly one *workspace*.  (A *workspace* can, of course, contain many *notes*.)

*Workspaces* contain one or more *users*, called "members".  Members may have different levels of access to a *workspace*, depending upon their specific "role".

Every *workspace* belongs to exactly one *organization*.

## Organizations

An ***organization*** is a collection of *users* and *workspaces*.

*Organizations* have a small number of core attributes:

 * `org_id` - an arbitrary, unique identifier for the *organization* (assigned).
 * `name` - a label or title for the *organization* (required).

*Organizations* contain zero or more *workspaces*.

*Organizations* contain one or more *users*, called "members".  Members may have different levels of access to an *organization* and the *workspaces* within it, depending upon their specific "role".
