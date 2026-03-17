1. # Dashboard

1) Pain Point: \[Dashboard epic\] helps users understand the spatial context of each site inside a single workspace before moving into deeper surveillance workflows. Users need to view the overall site map, recognize connected or disconnected sites, inspect the cameras of the displayed floor for a selected site, and decide whether to continue to \[Monitoring epic\] or move to \[Edge Agent topic\]. **(\[Epic\] help user solve pain points)**  
2) Objective:  
1. The user can view all available sites in the \[Map View Container\] inside the \[Main of Dashboard Page\].  
2. The user can distinguish site connection status and identify which site requires further inspection.  
3. The user can view the cameras belonging to the displayed floor of a connected site in the \[Side Sheet\].  
4. The user can use \[Dashboard epic\] as an entry point to the related \[Monitoring epic\] or \[Edge Agent topic\].  
3) Exit Epic:  
1. Click on \[Other Topic IconButton\] in \[Navigation Rail\] to go \[Other Topic Page\].  
2. Click on \[Other Epic Tab\] in \[TabBar\] to go \[Other Epic Page\].  
3. Click on \[User IconButton\] in \[Navigation Rail\], then select \[Log Out\] or \[User Setting\].  
4) Enter feature:  
1. \[Main of Dashboard Page\] landing on \[View Site Spatial Context feature\].  
2. Click on a connected site in the \[Map View Container\] to go \[View Cameras of Displayed Floor feature\].  
3. Click on \[View Detail Common Button\] in the \[Side Sheet\] to go \[Navigate from Dashboard to Related Monitoring Context feature\].

   1. #### *View Site Spatial Context*

1) Why: Without \[View Site Spatial Context feature\], \[Dashboard epic\] cannot help the user understand the overall spatial layout of available sites and determine which site should be inspected next. **(Without \[Features\], \[Epic\] cannot do "action" for the user.)**  
2) Feature purpose:  
1. Users can view all available sites in the \[Map View Container\] inside the \[Main of Dashboard Page\].  
2. Users can distinguish site status by color, with green for connected sites and red for disconnected sites.  
3. Users can identify which site or area requires further inspection before continuing to the next workflow.

       c)   Acceptance Scenarios:

1. **Given** the User is in \[Dashboard epic\]; **When** the \[Dashboard Page\] is loaded; **Then** the system displays the \[Map View Container\] in the \[Main of Dashboard Page\].  
2. **Given** the User is viewing the \[Map View Container\]; **When** the system loads available site data; **Then** the system displays all available sites on the map.  
3. **Given** the User is viewing the \[Map View Container\]; **When** a site has connection status data; **Then** the system displays the site in green if it is connected and in red if it is disconnected.  
4. **Given** the User is viewing the \[Map View Container\]; **When** the User reviews the displayed sites; **Then** the User can identify which site or area requires further inspection.  

   2. #### *View Cameras of Displayed Floor*

1) Why: Without \[View Cameras of Displayed Floor feature\], \[Dashboard epic\] cannot help the user preserve the current site context while reviewing which cameras belong to the floor currently displayed for a connected site. **(Without \[Features\], \[Epic\] cannot do "action" for the user.)**  
2) Feature purpose:  
1. Users can click a connected site in the \[Map View Container\] and open its \[Side Sheet\].  
2. Users can view the currently displayed floor information of the selected site in the \[Side Sheet\].  
3. Users can view the cameras that belong to the displayed floor in the \[Camera List Container\] while staying in the same \[Dashboard epic\].

       c)   Acceptance Scenarios:

1. **Given** the User is in \[Dashboard epic\] and viewing the \[Map View Container\]; **When** the User clicks a connected green site; **Then** the system displays the \[Side Sheet\] for the selected site.  
2. **Given** the \[Side Sheet\] is displayed for a connected site; **When** the system loads the selected site context; **Then** the system displays the displayed floor information and the \[Camera List Container\].  
3. **Given** the User is viewing the \[Side Sheet\] of a connected site; **When** the displayed floor is available; **Then** the system shows the cameras belonging to that displayed floor in the \[Camera List Container\].  
4. **Given** the User is viewing the \[Camera List Container\] of the displayed floor; **When** the User continues reviewing the site; **Then** the User can inspect floor-based cameras without leaving \[Dashboard epic\].  

   3. #### *Navigate from Dashboard to Related Monitoring Context*

1) Why: Without \[Navigate from Dashboard to Related Monitoring Context feature\], \[Dashboard epic\] cannot act as the entry point from spatial orientation into the next operational workspace for monitoring or device-related follow-up. **(Without \[Features\], \[Epic\] cannot do "action" for the user.)**  
2) Feature purpose:  
1. Users can review site-specific information in the \[Side Sheet\] after clicking a site in the \[Map View Container\].  
2. Users can see different context depending on site status: disconnected sites show \[No Signal status\] and a shortcut to \[Edge Agent topic\], while connected sites show displayed floor information, floor-based camera information, and a shortcut to \[Monitoring epic\].  
3. Users can move from \[Dashboard epic\] to the related monitoring context for deeper inspection.

       c)   Acceptance Scenarios:

1. **Given** the User is in \[Dashboard epic\]; **When** the User clicks a site in the \[Map View Container\]; **Then** the system displays site-specific information in the \[Side Sheet\].  
2. **Given** the User clicked a disconnected red site; **When** the \[Side Sheet\] is shown; **Then** the system displays the \[No Signal status\] and the \[Move to Edge Agent Common Button\].  
3. **Given** the User is viewing the \[No Signal status\] for a disconnected site; **When** the User clicks the \[Move to Edge Agent Common Button\]; **Then** the system navigates to the \[Edge Agent topic\].  
4. **Given** the User clicked a connected green site; **When** the \[Side Sheet\] is shown; **Then** the system displays the displayed floor information, the \[Camera List Container\] for that floor, and the \[View Detail Common Button\].  
5. **Given** the User is viewing a connected site in the \[Side Sheet\]; **When** the User clicks the \[View Detail Common Button\]; **Then** the system navigates to the related site monitoring view in the \[Monitoring epic\].  
