# Micro App - Plan Recommendation Engine Infrastructure
---
## **Overview**

### What is it ?

* Plan Recommendation Engine (PRE) is a module in AARP mediareplans /UHC Medicare solutions which provides recommendation of plans to enroll based on the user preferences.

* PRE allows the user to provide preferences in steps where each step in the flow represents one preference.

* PRE has two modules
  * PRE UI
  * PRE API

### System Flows - How is PRE Used
---
#### Flow 1: PRE flow
Users can launch PRE tool from AARP or UMS portals.

![PRE Micro App >> Infrastructure](../../img/pre/pre-seq-flow1.png)

---
#### Flow 2: Shopper profile flow
User launches PRE tool and lands on results page and on saving the recommendation results, the result will be stored in shopper profile DB which can be viewed later.

Users on launching Shopper profile UI, if the user had saved the PRE results, User can view the links for Edit my response and view results. On clicking this links,user will be redirected to PRE portal.

![PRE Micro App >> Infrastructure](../../img/pre/pre-seq-flow2.png)

---

#### Flow 3: PRE flow with shared data(Drugs/Providers)
User launches PRE tool and lands to Get Started page.

The drug details are populated to PRE from the local storage via DrugUserContextService, which contains the drug details stored by external tools like DCE/VPP.

The providers details are populated to PRE from the local storage, which contains the providers details stored by external tools like DCE/VPP.

Users on adding/removing/modifying the drugs in PRE Drugs page, the same will be updated in the local storage via DrugUserContextService, which will be used by external tools like DCE/VPP.

Users on adding/removing/modifying the providers in rally via PRE doctors page, the same will be updated in the local storage, which will be used by external tools like DCE/VPP.

![PRE Micro App >> Infrastructure](../../img/pre/pre-seq-flow3.png)

---

### PRE : Current flow of API calls
| On portal | Back-end does this |
| --- | --- |
| **Users enters zipcode on location page** | Invokes geo location api and fetches the zip and county details. On confirming the county details, invokes plan benefit api for MA/PDP/SNP plan details and invokes Medsupp Api for Medsupp plan details. |
| **Users on selecting Lookup doctors** | Redirects to Rally page and based on providers selection, updated the providers list. |
| **User enters partial drug name** | Invokes vendor's "Drug Autocomplete" with partial name, gets list of matching drug names. |
| **User chooses name from list** | Invokes vendor's "Drug Search" with chosen name, gets vendor's drug ID. Invokes vendor's "Drug Search" with vendor's drug ID, gets dosages and NDC's |
| **User on clicking next in drug page** | Invokes vendor's "Drug Pricing" api with vendor's plan ID, zip code, NDC's and Qty's, and default Pharmacy ID, gets drug cost for individual plans |
| **On loading Results page** | Invokes PRE Api with user selected preferences and plans and gets the ranked plans based on business rules. |

## **How it's deployed:**

### Network Architecture
![PRE Micro App >> Infrastructure](../../img/pre/cloud_network_arch_PRE.drawio.png)

### Apache web servers (Acquisition Portal web servers used for PRE)
<table>
    <tr>
                 <td> Environment </td>
                 <td> Region </td>
                 <td> Host </td>
                 <td> Host Name </td>
    </tr>
    <tr>
                 <td rowspan="2" align="center">Team-1 </td>
                 <td align="center">CentralUS </td>
                 <td>gpdacq-team-1-centralus </td>
                 <td>webrs2136.uhc.com </td>
    </tr>
    <tr>
                 <td rowspan="1" align="center">EastUS </td>
                 <td> gpdacq-team-1-eastus</td>
                 <td>webrs2137.uhc.com </td>
    </tr>
    <tr>
                 <td rowspan="2" align="center">Dev-1 </td>
                 <td align="center">CentralUS </td>
                 <td>gpdacq-dev-1-centralus </td>
                 <td>webrs2136.uhc.com </td>
    </tr>
    <tr>
                 <td rowspan="1" align="center">EastUS </td>
                 <td> gpdacq-dev-1-eastus</td>
                 <td>webrs2137.uhc.com </td>
    </tr>
    <tr>
                 <td rowspan="2" align="center">Qa-1 </td>
                 <td align="center">CentralUS</td>
                 <td>gpdacq-qa-1-centralus</td>
                 <td>webrs2138.uhc.com </td>
    </tr>
    <tr>
                 <td rowspan="1" align="center">EastUS </td>
                 <td>gpdacq-qa-1-eastus </td>
                 <td>webrs2139.uhc.com</td>
    </tr>
    <tr>
                 <td rowspan="2" align="center">Stage-1 </td>
                 <td align="center">CentralUS</td>
                 <td>gpdacq-stage-1-centralus</td>
                 <td>webrs2138.uhc.com </td>
    </tr>
    <tr>
                 <td rowspan="1" align="center">EastUS </td>
                 <td>gpdacq-stage-1-eastus </td>
                 <td>webrs2139.uhc.com</td>
    </tr>
    <tr>
                 <td rowspan="2" align="center">Prod-1 </td>
                 <td align="center">CentralUS</td>
                 <td>gpdacq-prod-1-centralus</td>
                 <td>webrs2138.uhc.com </td>
    </tr>
    <tr>
                 <td rowspan="1" align="center">EastUS </td>
                 <td>gpdacq-prod-1-eastus </td>
                 <td>webrs2139.uhc.com</td>
    </tr>
    <tr>
                 <td rowspan="4" align="center">Prod 1 </td>
                 <td rowspan="2" align="center">ELR </td>
                 <td>aqprd1web1</td>
                 <td>webrp2723.uhc.com </td>
    </tr>
</table>

## **API's:**

### API Environments
| Env | Back-end API | Link                                                                                                                                   |
| --- | --- |---|
| **Team** | pre-api | <https://plan-recommendation-engine-api-team-1.team.gpd-acq-azure.optum.com/pre-api/PlanRecommendationEngine/rest/recommend/plans>     |
| **Qa** | pre-api | <https://plan-recommendation-engine-api-qa-1.qa.gpd-acq-azure.optum.com/pre-api/PlanRecommendationEngine/rest/recommend/plans>         |
| **Dev** | pre-api | <http://preapi-prod-2.optum.com/PlanRecommendationEngine/rest/recommend/plans>                                                         |
| **Stage** | pre-api | <https://plan-recommendation-engine-api-stage-1.stage.gpd-acq-azure.optum.com/pre-api/PlanRecommendationEngine/rest/recommend/plans>   |
| **Prod** | pre-api | <https://plan-recommendation-engine-api-prod-2.prod.gpd-acq-azure.optum.com/pre-api/PlanRecommendationEngine/rest/recommend/plans>     |



### Which apps use PRE API's

| Consumer |  API's |
| --- |  --- |
| **PRE** | pre-api |


### Backend API dependencies
| Dependency | API's' | Link to spec |
| --- | --- | --- |
| **DCE** | Drug Search, Drug Dosage, Drug Autocomplete, Drug Pricing |
| **IS Medsupp Api** | GetMSPlans, GetMSBenefit, GetMSValueAddedServer |
| **Plan Benefit Api** | GetPlanBenefits | see Scrum team for details |
| **NA Plan Ranking Api** | NaPlanRanking  | see Scrum team for details |



### PRE Databases
| User | DB Type | Env | Database |
| --- | --- | --- | --- |
| **pre** | MySQL | Stage  | pre |
| **preadm** | MySQL | Stage | pre |
| **mnrp_app** | MySQL| Prod  | pre |
| **mnrp_app** | MySQL | Prod | pre |

## **Tools for the developer:**

### OCP  consoles
| Env | OCP URL                                                                 | Namespace |
| --- |-------------------------------------------------------------------------| --- |
| **Dev** | <https://ocp-elr-core-nonprod.optum.com>                                | digital-devv2 |
| **UAT/QA** | <https://ocp-elr-core-nonprod.optum.com>                                | digital-uatv2 |
| **Offline Stage** | <https://ocp-elr-dmz-stg.optum.com> <https://ocp-ctc-dmz-stg.optum.com> | mnr-acq-pre-stage-2 |
| **Online Stage** | <https://ocp-elr-dmz-stg.optum.com> <https://ocp-ctc-dmz-stg.optum.com> | mnr-acq-pre-stage-1 |
| **Offline Prod** | <https://ocp-elr-dmz.optum.com> <https://ocp-ctc-dmz.optum.com>         | mnr-acq-pre-prod-2 |
| **Online Prod** | <https://ocp-elr-dmz.optum.com> <https://ocp-ctc-dmz.optum.com>         | mnr-acq-pre-prod-1 |

### GitHub repositories
| API | Git Url                                                                    |
| --- |----------------------------------------------------------------------------|
| **pre-api** | <https://github.optum.com/gov-prog-digital/plan-recommendation-engine-api> |
| **medsupp-api** | <https://github.optum.com/gov-prog-digital/medsupp-api>                    |
| **mce-api** | <https://github.optum.com/gov-prog-digital/MedicalCostEstimationAPI>       |
| **pre-postback-api** | <https://github.optum.com/gov-prog-digital/pre-postback-data-api>          |

### Swagger specs
| Env | Link                                                                                                                                                                                                                  |
| --- |-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Stage-1** | <http://preapi-stage-1-ctc.optum.com/webjars/swagger-ui/index.html?configUrl=/v3/api-docs/swagger-config> , <http://preapi-stage-1-elr.optum.com/webjars/swagger-ui/index.html?configUrl=/v3/api-docs/swagger-config> |
| **Stage-2** | <http://preapi-stage-2-ctc.optum.com/webjars/swagger-ui/index.html?configUrl=/v3/api-docs/swagger-config> , <http://preapi-stage-2-elr.optum.com/webjars/swagger-ui/index.html?configUrl=/v3/api-docs/swagger-config> |
| **Prod-1** | <http://preapi-prod-1-ctc.optum.com/webjars/swagger-ui/index.html?configUrl=/v3/api-docs/swagger-config> , <http://preapi-prod-1-elr.optum.com/webjars/swagger-ui/index.html?configUrl=/v3/api-docs/swagger-config>   |
| **Prod 2** | <http://preapi-prod-2-elr.optum.com/webjars/swagger-ui/index.html?configUrl=/v3/api-docs/swagger-config> , <http://preapi-prod-2-ctc.optum.com/webjars/swagger-ui/index.html?configUrl=/v3/api-docs/swagger-config>   |

### CI/CD jobs
| API | Job Url                                                                                                            |
| --- |--------------------------------------------------------------------------------------------------------------------|
| **pre-api** | <https://jenkins-gov-prog-digital.origin-elr-core-nonprod.optum.com/job/build/job/plan-recommendation-engine-api/> |
| **medsupp-api** | <https://jenkins-gov-prog-digital.origin-elr-core-nonprod.optum.com/job/build/job/medsupp-api/>                    |
| **mce-api** | <https://jenkins-gov-prog-digital.origin-elr-core-nonprod.optum.com/job/build/job/MedicalCostEstimationAPI/>       |
| **pre-postback-api** | <https://jenkins-gov-prog-digital.origin-elr-core-nonprod.optum.com/job/build/job/pre-postback-data-api/>          |

<!-- Sample markdown to embed PDF
<object data="pdf/TMDB-2901404-UCP_Member_Acquisition_PRJ186072_10032019.pdf" type="application/pdf" width="700px" height="700px">
    <embed src="pdf/TMDB-2901404-UCP_Member_Acquisition_PRJ186072_10032019.pdf">
        <p>This browser does not support PDFs. Please download the PDF to view it: <a href="pdf/TMDB-2901404-UCP_Member_Acquisition_PRJ186072_10032019.pdf">Download PDF</a>.</p>
    </embed>
</object>
-->
