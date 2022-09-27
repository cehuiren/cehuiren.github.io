---
title: OpenRoads Designer Project File Management
author: 网络转载
date: 2013-10-05 18:25:00 +0800
categories: OpenRoads 综合基础
tags: 文件 管理 en
excerpt: 
---
* content
{:toc}

# Project Management
Project Management can mean a lot of things.But for the purposes of this presentation,it means **managing your project data so that OpenRoads Designer can operate in the most effective and efficient manner possible.**

## Preface
- "Best Practices" are suggestions and good "rules of thumb" to go by,**not requirements**.
- There is no one size fits all.
- Processes can and do vary by:
 - Organization
 - Project
 - Discipline

## Agenda
- Seed Files
- Data Segregation
- Reference Files

## Seed Files
### Make sure they are clean!
- No previous civil data
- No holdovers(e.g. line styles,fonts,etc.) from previous versions

## GeoCoordinate Systems
- Assign coordinate system for the project
 - Allows you to easily consume other similar data (e.g. DEM files)
 - Allows you to easily integrate with other software(e.g. Google Earth)

### 2D and 3D Seed Files
- 3D for Survey and Terrains
- 2D for all others (Geometry,Corridors,Superelevation,etc.)
 - Let ORD create and manage the 3D Models

## Data Segregation
- Topo/Survey
- Terrain
- Geometry
- Corridors
- Superelevation
- Utilities
- Cross Sections
- Plan-Profile Sheets
- Drainage
- Bridges
- Geotech
- Control Features
- Proposed Terrains
- Etc.

### Why?
- Smaller files are inherently faster and more efficient.
- Easier to manage and recall where things are.
- More control later wher you need to compile then for diffenrent scenarios (e.g. create a composite model for LumenRT).

### Common Mistakes
- Creating 'linked' objects in the Corridor file
  - Example:Let's say you create a proposed terrain from the finish grade mesh.If you create it in the same file as the corridor,then it will be updated every time you process the corridor.
  - Example:Let's say you create civil cells for driveways in the corridor file.Then every time you process the corridor it has to update the civil cells.
  - Place these 'linked' objects in their own file.
- Creating extraneous data in the Corridor file
  - Superelevation
  - Cross Sections
  - Place these in their own file

### Alignments - To segregate individually or not?
- May be warranted if multiple team members want to work on diffenrent alignments at the same time.
- If using ProjectWise,would allow you to 'check out' alignments
- Can use a blank "master" file to get all geometry at once.
  - √ Create blank master file
  - √ Reference each individual alignment dgn
  - √ Reference master file when you want all geometry

### Corridors - Break them up or not?
- Rule of thumb? Yes.
- Why?
  - √ Not because of any size/memory restrictions
  - √ Allows multiple team members to work at same time.
  - √ Smaller sections are faster and easier to work with.
  - √ You can still reference together when you need to see or utilize the project as a whole.

### Corridors - What dictates where to break it?
- This can be diffenrent from project to project.
  - √ Based on engineering configuration (e.g interchanges)
  - √ Based on size/scale of the project
  - √ Based on structures (e.g. bridge locations)
  - √ Based on geographical entities (e.g. intersections)

### Awareness
Whatever you decide to do,make sure your team is aware up-front of the plan,so everyone is on the same page.

## Reference Files - Live Nesting
### Live Nesting
- A file can have any number of reference attachments,and those attachments can have attachments,which in turn can have more attachments,and son and so on.
- Enabling **Live nesting** when referencing a file causes it's children (and potentially their children) to be automatically referenced as well.
- To what 'depth' these children are located depends on the **Nesting Depth** value that you use.

Q:What's the Advantage?

A:Faster and more efficient method of attaching and displaying multiple files,just by attaching a single,parent reference to a model.

Q: Are duplicate references a disadvantage?

A: No.Duplicate references are recognized and ignored.