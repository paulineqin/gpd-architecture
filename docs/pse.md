# Micro App - In-Page Provider Search Infrastructure
---
##**Overview:**##

### What is it ?

The In-Page Provider Search Experience (PSE) gives consumers a provider search capability within the AMP/UMS domain, for looking up a provider and determining non-plan specific or plan-specific in and out of network status. Previously, in order to look up a provider, the user was taken off the AMP/UMS page to a separate page for a Rally UI experience. The resulting provider choice was sent back to the AMP/UMS domain/page and the Rally UI page closed down. The PSE micro app enables to user to stay within the same AMP/UMS UI experience to look up their providers.<br/>

The PSE micro app has two main components:<br/>

1) PSE UI<br/>
2) PSE basic API<br/>

The PSE micro app also depends upon other systems such as Optum Digital API, but they are not considered part of the PSE micro app because they are either owned by an outside company or they are not part of the core PSE functionality.

**Note** :- 
!!! note
    **PSE Micro app is built on XYZ...**

#### High-level Architecture
![PSE Micro App >> Infrastructure](../../img/pse/PSE_UI_Drawer.png)

### System Flows - How is PSE Used
- - - - - - - - - -
#### Within OLE: "Enhance Pre-Selected Providers for PSE" flow
![PSE Micro App >> Conceptual Diagram >> Part 1]../..(/img/pse/PSE_diagram_enhance_providers_in_OLE.png)
- - - - - - - - - -
#### Within Telesales: "Send Enhanced Pre-Selected Providers to Salesforce" flow
![PSE Micro App >> Conceptual Diagram >> Part 2](../../img/pse/PSE_diagram_send_enhanced_providers_to_Telesales.png)
- - - - - - - - - -
#### Within PSE: "Pageload for OLE-launched PSE" flow
![PSE Micro App >> Conceptual Diagram >> Part 3](../../img/pse/PSE_diagram_drawer_pageload_from_OLE.png)
- - - - - - - - - -
#### Within VPP, PRE, or VP: "Open the PSE drawer" flow
![PSE Micro App >> Conceptual Diagram >> Part 4](../../img/pse/PSE_diagram_open_drawer_from_VPP_PRE_VP.png)
- - - - - - - - - -
#### Within PSE: "Pageload for VPP-PRE-VP-launched PSE" flow
![PSE Micro App >> Conceptual Diagram >> Part 5](../../img/pse/PSE_diagram_drawer_pageload_from_VPP_PRE_VP.png)


### PSE : Current flow of API calls
table

##**How it's deployed:**##

### Network Architecture

#### Online and Offline Flow
diagram

#### Online Flow
diagram

#### Offline Flow
diagram

### Apache web servers (Acquisition Portal web servers used for PSE)
table

##**API's:**##

### API Environments
table


### Which apps use PSE API's
table

### Which apps use PSE UI as common-tool
note

### Backend API dependencies
table

### PSE Databases
table

##**Tools for the developer:**##

### Azure consoles
table

### GitHub repositories
- refer to Scrum team or A-team document

### Swagger specs
table

### CI/CD jobs
- refer to Scrum team or A-team document

<!-- Sample markdown to embed PDF
<object data="pdf/TMDB-2901404-UCP_Member_Acquisition_PRJ186072_10032019.pdf" type="application/pdf" width="700px" height="700px">
    <embed src="pdf/TMDB-2901404-UCP_Member_Acquisition_PRJ186072_10032019.pdf">
        <p>This browser does not support PDFs. Please download the PDF to view it: <a href="pdf/TMDB-2901404-UCP_Member_Acquisition_PRJ186072_10032019.pdf">Download PDF</a>.</p>
    </embed>
</object>
-->
