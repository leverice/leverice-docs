.. meta::
  :description: This document contains all the technical information about app descriptors.

.. _app-desc-reference-label:

App descriptor
================

Fields
########

**String id (required)** - application id. Must be unique.

**int version (required)** - version of your app uses for the data migration

**FacetDescriptor[] facets (required)** - your app facet descriptors

**PluginSettingsDescriptor settings (optional)** - used to add your app to the channel settings app management

FacetDescriptor
################

**String id (required)** - facet id. Must be unique in the app scope.

**String name (required)** - facet name.

**FullFacetId[] requires (optional)** - required facets to add to the channel with this facet. Can be empty.

**FullFacetId[] requireAlreadyAddedFacets (optional)** - required facets to be added before this facet added to the channel.

**String iconName (required)** - icon for the channel with this facet. Pattern: **"plugin:${appId}:${assetFileName}.svg"**

**String[] directCommands (optional)** - commands available to run with "/appId.facetId:command" from any channel in the workspace

PluginSettingsDescriptor
##########################

**boolean activateAfterInstallation (required)** - should we activate app after install or not.

**boolean allowMultipleInstallation (required)** - whether multiple installation restricted.

**String[] allowedFacetWildcards (optional)** - facets patterns to include this app to the channel settings.
Pattern **${appId}.${facetId}**. "*" instead of any id means 'all' plugins or facet.

**String[] forbiddenFacetWildcards (optional)** - facets patterns to exclude this app from the channel settings.
Pattern **${appId}.${facetId}**. "*" instead of any id means 'all' plugins or facet.

**Map<String:status, SettingFormDescriptor[]> forms (required)** - forms to show in channel settings for one of four statuses (notInstalled, installed, activated, unsorted)

SettingFormDescriptor
######################

**String[] allowedFacetWildcards (optional)** - facets patterns to include this app to the channel settings.
Pattern **${appId}.${facetId}**. "*" instead of any id means 'all' plugins or facet.

**String[] forbiddenFacetWildcards (optional)** - facets patterns to exclude this app from the channel settings.
Pattern **${appId}.${facetId}**. "*" instead of any id means 'all' plugins or facet.

**String visibilityCondition (required)** - facets patterns to exclude this app from the channel settings.

**String icon (required)** - icon to show in the channel settings. Pattern: **"plugin:${appId}:${assetFileName}.svg"**

**String description (required)** - description to show in the channel settings.

**DialogWindowComponentDto[] items (required)** - frontend components to build settings form







