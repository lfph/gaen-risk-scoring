# Configuring Exposure Notification Risk Scores for COVID-19

## Guidance from the Risk Score Symposium Invitational

  * [Introduction](#introduction)
  * [Structure of the risk score](#structure-of-the-risk-score)
  * [Thresholds and weights](#thresholds-and-weights)
    + [Background](#background)
    + [Updated thresholds and weights recommendations](#updated-thresholds-and-weights-recommendations)
  * [Infectiousness curve](#infectiousness-curve)
    + [Updated infectiousness curve recommendations](#updated-infectiousness-curve-recommendations)
  * [Tiered notifications](#tiered-notifications)
    + [Examples of tiered notification messaging](#examples-of-tiered-notification-messaging)
    + [A proposed path forward for messaging evaluation](#a-proposed-path-forward-for-messaging-evaluation)
  * [Risk Score updating over time](#risk-score-updating-over-time)
  * [Contributors](#contributors)
  * [Notes](#notes)

## Introduction

Exposure notifications are phone-based alerts that people can receive to inform them that they’ve been exposed to someone diagnosed with COVID-19. The Google-Apple Exposure Notification system (GAEN) has configurable components, including a risk score, which allows health authorities to specify which types or levels of exposures should trigger a notification, and the user-facing messages displayed in the notification.

In a series of virtual meetings in November 2020, the Risk Score Symposium Invitational brought together specialists and experts from multiple disciplines who were actively involved in GAEN implementation and/or research. The symposium’s purpose was to inform the decision-making process for health authorities who are using, or plan to use, GAEN in their region. The symposium reconvened in August 2021 to review the recommendations in the context of new information about the Delta variant and the changing landscape of vaccination uptake and test availability.

Below, we provide risk score parameter guidance based on currently available evidence. These recommendations are intended to be shared and open sourced, so the larger community can contribute to and improve upon these recommendations.

The authors recognized that the Delta variant warranted an updated infectiousness curve. Those familiar with the v1 recommendations may remember that the “narrow” and “wide” nets each combined a recommended infectiousness curve with the attenuation-duration settings.  Below, the recommendation is to decouple the attenuation-duration settings from the infectiousness schedule and weights, as proximity and infectiousness are independent variables. The v2 recommendations for “narrow” and “wide” attenuation-duration settings have been updated as well, based on risk score models and [Exposure Notification Privacy-preserving Analytics](https://covid19-static.cdn-apple.com/applications/covid19/current/static/contact-tracing/pdf/ENPA_White_Paper.pdf) (ENPA)[^1] efficacy data available at the time of the reconvening.

Another notable change in v2 is the suggestion to consider whether a tiered approach to notifications may better serve the individual’s need for risk awareness and/or the needs of a public health authority (PHA), by providing notice that sub-threshold encounters have occurred. This may encourage continued “best practices” by individuals as social and travel restrictions ease. Tiered thresholds also provide additional visibility into ENPA metrics for public health teams, even if the same user-facing message is assigned to each notification category.

This guidance should be viewed as a starting point; each public health authority will need to make their own decisions tailored to the communities they are serving.


## Structure of the risk score

The risk score estimates, in minutes, the degree of relevant exposure to someone with a COVID-19 diagnosis. It is calculated by multiplying two<sup><a href="https://github.com/lfph/gaen-risk-scoring/blob/main/risk-scoring.md#user-content-fn-1-f213d0cd284e2fc498b041daf93bd544">1</a></sup> weighted terms together: (1) duration at Bluetooth attenuation and (2) infectiousness of the index patient based on days since symptom onset. In the case of an asymptomatic individual, the date of test is used instead of date of symptom onset. Further documentation on the structure of the risk score and how it is calculated (including a worked example) is available from [Apple](https://developer.apple.com/documentation/exposurenotification/enexposureconfiguration) and [Google](https://developers.google.com/android/exposure-notifications/meaningful-exposures) technical specification documents.



![Risk score overview](/img/risk-score-overview.png "Risk scores consist of attenuation, infectiousness weight, and (optionally) report type")


[^2]


## Thresholds and weights 


### Background

In November 2020, the Risk Score Consortium convened and recommended two sets of parameters for the Thresholds and Weights associated with infectious encounters:  a narrow net and a wide net. The recommendations were as shown below:



<table>
 <caption>Table: v1 threshold and weight recommendations</caption>
  <tr>
   <td>
   </td>
   <td>
   </td>
   <td>
<strong>Immediate</strong>
   </td>
   <td><strong>Near</strong>
   </td>
   <td><strong>Medium</strong>
   </td>
   <td><strong>Other</strong>
   </td>
  </tr>
  <tr>
   <td rowspan="2" ><strong>Narrower Net 1.0</strong>
   </td>
   <td><em>Threshold</em>
   </td>
   <td>&lt;55 dB
   </td>
   <td>&lt;63 dB
   </td>
   <td>&lt;70 dB
   </td>
   <td>--
   </td>
  </tr>
  <tr>
   <td><em>Weight</em>
   </td>
   <td>150%
   </td>
   <td>100%
   </td>
   <td>40%
   </td>
   <td>0%
   </td>
  </tr>
  <tr>
   <td rowspan="2" ><strong>Wider Net 1.0 </strong>
   </td>
   <td><em>Threshold</em>
   </td>
   <td>&lt;55 dB
   </td>
   <td>&lt;70 dB
   </td>
   <td>&lt;80 dB
   </td>
   <td>--
   </td>
  </tr>
  <tr>
   <td><em>Weight</em>
   </td>
   <td>200%
   </td>
   <td>100%
   </td>
   <td>25%
   </td>
   <td>0%
   </td>
  </tr>
</table>

Since that time, a variety of jurisdictions have implemented both sets of recommendations, as well as some in-between. Nearly a year later, we now have the benefit of measuring the efficacy outcomes for these two sets of recommendations. Generally speaking, the results for Narrow Net show substantially fewer exposure notifications going out - to the degree that we now regard it as too narrow for most circumstances and therefore less effective.


### Updated thresholds and weights recommendations

The evolution of the highly infectious Delta variant also suggests an adjustment to the Narrow Net recommendations is needed to keep pace with the changing characteristics of the virus. We propose to widen the recommended “narrow net” settings for thresholds and weights to a new set of parameters referred to as “Narrow Net v2”.  This corresponds with the updated transmission curve in the context of the Delta variant.  

The proposed updated values for Threshold and Weight for Narrow Net v2 shown in the table below are at the mid-point between the former Narrow Net v1 and the existing Wide Net. We continue to recommend the Wide Net as a potential option for all jurisdictions. 

<table>
 <caption>Table: v2 threshold and weight recommendations</caption>
  <tr>
   <td>
   </td>
   <td>
   </td>
   <td><strong>Immediate</strong>
   </td>
   <td><strong>Near</strong>
   </td>
   <td><strong>Medium</strong>
   </td>
   <td><strong>Other</strong>
   </td>
  </tr>
  <tr>
   <td rowspan="2" ><strong>Narrow Net v2</strong>
   </td>
   <td><em>Threshold</em>
   </td>
   <td>≤55 dB
   </td>
   <td>≤67 dB
   </td>
   <td>≤75 dB
   </td>
   <td>--
   </td>
  </tr>
  <tr>
   <td><em>Weight</em>
   </td>
   <td>175%
   </td>
   <td>100%
   </td>
   <td>33%
   </td>
   <td>0%
   </td>
  </tr>
  <tr>
   <td rowspan="2" ><strong>Wide Net</strong>
   </td>
   <td><em>Threshold</em>
   </td>
   <td>≤55 dB
   </td>
   <td>≤70 dB
   </td>
   <td>≤80 dB
   </td>
   <td>--
   </td>
  </tr>
  <tr>
   <td><em>Weight</em>
   </td>
   <td>200%
   </td>
   <td>100%
   </td>
   <td>25%
   </td>
   <td>0%
   </td>
  </tr>
</table>



## Infectiousness curve

We recommend adjusting the infectiousness curve to reflect current knowledge about the Delta variant’s viral load ([source](https://www.medrxiv.org/content/10.1101/2021.09.28.21264260)) over time and the resultant change in infectiousness. As more variants of concern come into play we recommend re-visiting this infectiousness curve to reflect the latest research.

For v1 this group recommended different symptom onset maps for each net.

<table>
 <caption>Table: v1 symptom onset map</caption>
  <tr>
   <td>
   </td>
   <td>
<strong>Narrower net</strong>
   </td>
   <td><strong>Wider net</strong>
   </td>
  </tr>
  <tr>
   <td>Symptom Onset Map 
   </td>
   <td><em>None</em>: days -14 to -4; days +5 to +14
<p>
<em>Standard</em>: day -3; day +4
<p>
<em>High</em>: days -2 to +3
   </td>
   <td><em>None</em>: days -14 to -6; days +10 to +14 
<p>
<em>Standard</em>: -5 to -4 days; days +5 to +9 
<p>
<em>High</em>: days -3 to +4
   </td>
  </tr>
  <tr>
   <td>Standard weight[^4]
   </td>
   <td>30%
   </td>
   <td>60%
   </td>
  </tr>
  <tr>
   <td>High weight[^4]
   </td>
   <td>100%
   </td>
   <td>200%
   </td>
  </tr>
</table>

### Updated infectiousness curve recommendations

For v2 this has been consolidated based on what we now know about the transmission of COVID-19. This leads to the same infectiousness curve no matter whether employing a narrow or wide net for the Thresholds and Weights.

<table>
 <caption>Table: v2 symptom onset map</caption>
  <tr>
   <td><strong>Day</strong>
   </td>
   <td><strong>-14</strong>
   </td>
   <td><strong>-13</strong>
   </td>
   <td><strong>-12</strong>
   </td>
   <td><strong>-11</strong>
   </td>
   <td><strong>-10</strong>
   </td>
   <td><strong>-9</strong>
   </td>
   <td><strong>-8</strong>
   </td>
  </tr>
  <tr>
   <td>Infectiousness
   </td>
   <td>N
   </td>
   <td>N
   </td>
   <td>N
   </td>
   <td>N
   </td>
   <td>N
   </td>
   <td>N
   </td>
   <td>N
   </td>
  </tr>
  <tr>
   <td><strong>Day</strong>
   </td>
   <td><strong>-7</strong>
   </td>
   <td><strong>-6</strong>
   </td>
   <td><strong>-5</strong>
   </td>
   <td><strong>-4</strong>
   </td>
   <td><strong>-3</strong>
   </td>
   <td><strong>-2</strong>
   </td>
   <td><strong>-1</strong>
   </td>
  </tr>
  <tr>
   <td>Infectiousness
   </td>
   <td>N
   </td>
   <td>N
   </td>
   <td>N
   </td>
   <td>S
   </td>
   <td>S
   </td>
   <td>H
   </td>
   <td>H
   </td>
  </tr>
  <tr>
   <td><strong>Day</strong>
   </td>
   <td><strong>0</strong>
   </td>
   <td><strong>1</strong>
   </td>
   <td><strong>2</strong>
   </td>
   <td><strong>3</strong>
   </td>
   <td><strong>4</strong>
   </td>
   <td><strong>5</strong>
   </td>
   <td><strong>6</strong>
   </td>
  </tr>
  <tr>
   <td>Infectiousness
   </td>
   <td>H
   </td>
   <td>H
   </td>
   <td>H
   </td>
   <td>H
   </td>
   <td>S
   </td>
   <td>S
   </td>
   <td>N
   </td>
  </tr>
  <tr>
   <td><strong>Day</strong>
   </td>
   <td><strong>7</strong>
   </td>
   <td><strong>8</strong>
   </td>
   <td><strong>9</strong>
   </td>
   <td><strong>10</strong>
   </td>
   <td><strong>11</strong>
   </td>
   <td><strong>12</strong>
   </td>
   <td><strong>13+</strong>
   </td>
  </tr>
  <tr>
   <td>Infectiousness
   </td>
   <td>N
   </td>
   <td>N
   </td>
   <td>N
   </td>
   <td>N
   </td>
   <td>N
   </td>
   <td>N
   </td>
   <td>N
   </td>
  </tr>
</table>



<table>
  <tr>
   <td>Standard weight
   </td>
   <td>30%
   </td>
  </tr>
  <tr>
   <td>High weight
   </td>
   <td>100%
   </td>
  </tr>
</table>



## Tiered notifications

Tiered notifications, which allow different responses for different levels of a risk score, offer two key benefits. First, the ability to give users a message appropriate to their potential level of exposure, and second, allow system operators to get better analytics around different subgroups of users. 

The German exposure notification tool, Corona-Warn-App, has successfully implemented this approach using Risk Cards that provide users with exposure-specific guidance on a scale from low risk with no exposures to an increased risk (see images below). These Risk Cards provide information on protective behaviors, symptom monitoring, testing, and quarantine/isolation, to inform users’ decision-making around protecting themselves and their community. A similar strategy can be utilized with tiered Exposure Notification messaging linked to different risk scores with varying configuration settings. Proposed levels for tiered notifications are as follows:



* **“Advisory”**: An individual was near someone who tested positive but does not meet current PHA, CDC, or WHO guidance as a close contact. This level could be used to indicate a potential “subthreshold” exposure, which could occur with exposure to a variant of concern that is more transmissible than the original COVID-19 strain. Guidance distributed in the message would include monitoring for symptoms, getting tested if symptoms develop or are present, and getting a vaccine if not already vaccinated.
* **“Alert”**: An individual was near someone who tested positive and meets current PHA, CDC, or WHO guidance as a close contact. The recommended approach is to maintain current exposure notification messaging, highlighting protective behaviors and isolation if the individual is fully vaccinated or not fully vaccinated, and guidance on next steps if the individual has or develops symptoms, including potential prophylactic treatments~~.~~

The exposure notification content should be designed to encourage users to take action and navigate to the PHA-provided webpage with full guidance. The recommended approach is to separate actionable key messages in a brief, clear, bulleted list format. Incorporating colors and visuals can also improve message clarity and ensure users quickly understand next steps to take to protect themselves and others.

### Examples of tiered notification messaging


<table>
  <tr>
   <td><strong>Advisory Notification</strong>
   </td>
   <td><strong>Alert Notification</strong>
   </td>
  </tr>
  <tr>
   <td>A person you were near within the last 14 days has tested positive for COVID-19.
<ul>
<li>Your exposure did not meet the current [Health authority] guidance for a close contact.
<li>Continue to be safe with physical distancing, wearing a mask, and monitoring for symptoms.
<li>If you have or develop COVID-19 symptoms, regardless of vaccination status, isolate yourself from others immediately and get tested.</li></ul>
    <p>Click the link for further guidance.</p>

   </td>
   
   <td><p>A person you were near within the last 14 days has tested positive for COVID-19.</p>
  <p>If you are fully vaccinated:</p>
<ul>
 <li>Get tested 5-7 days after the date of your exposure.</li>
 <li>Wear a mask indoors for 14 days after exposure. </li></ul>
<p>
If you are NOT fully vaccinated:
<ul>
 <li>Isolate yourself from others and get tested as quickly as possible. </li>
 <li>If your test is negative, get tested again 5-7 days after the date of exposure. </li></ul>
<p> If you have COVID-19 symptoms, regardless of vaccination status, isolate yourself from others immediately and get tested.</p>
<p>
 Click the link for further guidance.</p>
</li>
</ul>
</li>
</ul>
   </td>
  </tr>
</table>


**The examples above are a starting point for public health authorities to craft messaging appropriate for their communities and situational needs.**

Current Health Authority guidance states that a close contact is someone who was within six feet of an infected person (laboratory-confirmed or a clinically compatible illness) for a cumulative total of 15 minutes or more over a 24-hour period.[^3],[^4] As of this writing, there is currently no guidance from the CDC or WHO regarding exposures of a shorter duration or proximity, and we recognize that Bluetooth proximity and risk score classifications are imperfect proxies for biological transmission risk. However, even by using signal strength/duration proxy for the “six foot, 15 minute” exposure, studies have shown that variants of concern exhibit increased transmissibility that could be missed by current risk score classifications.[^5],[^6],[^7] Adding an advisory level notification of possible exposure would allow users to be informed of potential exposures of which they may otherwise be unaware. This could have the added benefit of not only reaching more close contacts, but could also remind users that exposure notifications provide an additional layer of protection as social distancing restrictions are lifted.

Public health authorities may also find additional utility in using multiple classifications for expanded disease transmission analytics, while sending identical user-facing messaging. This allows PHAs to understand how many people receiving “alert” messages would be “close” to the advisory/alert threshold and how many might be more solidly in the “alert” category. The table below illustrates how they could send the same notification at different thresholds in order to collect this data. Configurations, including duration, can be adjusted to provide aggregated data regarding differences in exposure details. Although there are barriers and limitations in using aggregated data for evaluation work, this could potentially support analyses of the impact of exposure notifications to mitigate the spread of COVID-19. Exposure notification messaging can subsequently be kept the same across varying classifications to ensure continuity of messaging for users who meet baseline criteria. 

<table>
 <caption>Table: Examples of multiple classifications for analytics</caption
  <tr>
   <td><strong>Classification</strong>
   </td>
   <td><strong>Threshold 1</strong>
   </td>
   <td><strong>Threshold 2</strong>
   </td>
   <td><strong>Threshold 3</strong>
   </td>
   <td><strong>Threshold 4</strong>
   </td>
  </tr>
  <tr>
   <td><strong>Exposure risk value (minutes)</strong>
   </td>
   <td>&lt; 5
   </td>
   <td>5-15
   </td>
   <td>15-30
   </td>
   <td>30
   </td>
  </tr>
  <tr>
   <td><strong>Notification Provided</strong>
   </td>
   <td>None
   </td>
   <td>Advisory
   </td>
   <td>Alert
   </td>
   <td>Alert
   </td>
  </tr>
</table>


Example messages from German Corona-Warn app:

![Corona Warn example with no exposures](/img/CWA_no_exposures.png)
![Corona Warn example with low risk level](/img/CWA_risk_level_low.png)
![Corona Warn example with high risk level](/img/CWA_risk_level_high.png)
[^8]

### A proposed path forward for messaging evaluation 

Recent studies have been conducted or are currently underway examining the impact of public health and governmental communication strategies in the context of COVID-19 message framing and content. These evaluations have examined the impact of using gain-framed (attaining a desired outcome) vs. loss-framed (attaining an undesirable outcome) messages to encourage engagement in protective behaviors in self and others (such as hand washing, physical distancing, limiting activities outside of the household) and increasing awareness of risks. For example, Gantiva et al (2021)[^9] reported that gain-framed messages were more effective to motivate self-care behaviors (hand washing and staying home) while loss-framed messages were more effective at increasing awareness of risk of contagion. In studies of populations where risk perception is usually low, Dryhurst et al (2020)[^10] and Lohiniva et al (2020)[^11][[8]](https://stateofwa.sharepoint.com/sites/DOH-OutbreakandResponseCross-sectionalProjects/Shared%20Documents/WA%20Notify%20(EN)/Communications/Linux%20Foundation%20risk%20scoring%202.0/LFPH%20Tiered%20Notifications%20Language%20Draft%2010.13.21_reformatted.docx#_edn7) found that loss-framed messages were significantly correlated with reported adoption of self-care behaviors to mitigate COVID-19 contagion. Other COVID-19 messaging evaluations have reported: highlighting individuals' responsibilities to one another increased adopting protective behaviors[^12]; including suggestions on how to make self-isolation easier improved message effectiveness[^13]; and emphasizing consequences of non-compliance for the individual or family (vs. unknown others) were more effective in motivating compliance.[^14]

However, evaluations to date have not focused on the framing or content of digital exposure notification messages smartphone users will receive nor on the message source which may influence the recipient's assessment of message authority or provenance. We propose developing a plan to test exposure notification message framing, source/authority, and content (such as, testing of minimal vs. denser messages, use of color, emoticons and/or images like stop lights) in a field study. In addition to testing with representative general population groups, diverse population groups would be included, such as: vulnerable populations; communities disproportionately impacted by COVID-19; populations known to perceive their risk as low; lower socio-economic status; etc. Evaluating users' actual responses to different message configurations will help inform how to frame an exposure notification so that the recipient is sufficiently motivated to click on the link in the message and access actionable public health information and recommendations around protective behaviors and getting tested. 


## Risk Score updating over time

A deployment of Exposure Notifications, like other public health interventions, should involve monitoring key indicators to best drive program improvements. The epidemiology of COVID-19 is constantly changing over time and an ongoing practice monitoring and improving a deployment can drive continuous efficacy in the context of this ongoing change. Jurisdictions should continually monitor these metrics in order to improve upon their settings to maximize the number of accurate notifications sent, while attempting to also minimize false positives to the best of their ability.

Continuous monitoring and improvement should be based on a regularly updated understanding of:

* The epidemiology of COVID-19 within a jurisdiction
* The desired level of public health interventions, both in terms of positive health outcomes (reducing spread of COVID-19) and negative externalities (unnecessary quarantines)
* Specific indicators about the efficacy of a given deployment, such as:
    * **Number of Exposure Notifications sent per key upload:** 
        * How many individuals your deployment notifies given an individual who shares their positive status
        * The degree to which social activity is “normal” vs “locked down”
        * The degree to which the contacts of app users are also app users
    * **EN Secondary Attack Rate:** the number of users who get an Exposure Notification up to 14 days before using a verification code per the number of Exposure Notifications sent
        * This measures the “hit rate” of your deployment. This is an estimation of how many users who get notifications will actually test positive
        * As much as possible, this should be linked to data about what attenuation and duration settings were enabled for those notifications in order to continue feeding data back into the system around updating the various Risk Score settings.
    * **Key upload rate:** number of verification codes used per # of key uploads
        * This measures of the users who have the opportunity to share their public status and contribute to the system, how many actually do so.

This symposium reconvened in the context of new information about the epidemiology of COVID-19, and we similarly believe there is value in regularly revisiting a given deployment’s risk score settings to evaluate whether any changes would yield program improvements. Public health authorities can make meaningful adjustments to a key intervention with a relatively small and infrequent amount of effort; this is an opportunity provided by the flexibility and reach of Exposure Notifications that should not be overlooked.


## Contributors

We would like to recognize all participants in the Risk Score Symposium Invitational for their contributions (listed alphabetically):

_Eliah Aronoff-Spencer, CA Notify / UC San Diego_

_Justus Benzler, Robert Koch Institute, Berlin, Germany_

_Todd Burner, Google_

_Christophe Fraser, University of Oxford, UK_

_Amanda Higgins, Washington State Department of Health_

_Bryant Thomas Karras, Washington State Department of Health_

_Andrea King, Washington State Department of Health_

_William B. Lober, University of Washington_

_Randy Marsden, Apple_

_Debra Revere, University of Washington (WA Notify)_

_Curran Schiefelbein, MIT Lincoln Laboratory_

_Jenny Wanger, Linux Foundation Public Health_

_Gregory Zane, Washington State Department of Health_

_Sam Zimmerman, Pathcheck_


## Notes

[^1]:
     More information on the ENPA system can be found at  https://covid19-static.cdn-apple.com/applications/covid19/current/static/contact-tracing/pdf/ENPA_White_Paper.pdf.

[^2]:
     Image source: [https://developer.apple.com/documentation/exposurenotification/enexposureconfiguration](https://developer.apple.com/documentation/exposurenotification/enexposureconfiguration)

[^3]:
     Centers for Disease Control and Prevention – [Appendices](https://www.cdc.gov/coronavirus/2019-ncov/php/contact-tracing/contact-tracing-plan/appendix.html). Last Updated October 19, 2021.

[^4]:
     World Health Organization –[ Contact tracing in the context of COVID-19](https://www.who.int/publications/i/item/contact-tracing-in-the-context-of-covid-19). Last updated February 1, 2021.

[^5]:
     Xiao J, Kang M. Transmission Dynamics of an Outbreak of the COVID-19 Delta Variant B.1.617.2 – Guangdong Province, China, May – June 2021. doi: [10.46234/ccdcw2021.148 ](https://www.researchgate.net/profile/Jianpeng-Xiao/publication/352931648_Transmission_Dynamics_of_an_Outbreak_of_the_COVID-19_Delta_Variant_B16172_-_Guangdong_Province_China_May_-_June_2021/links/60dfcde4a6fdccb745ffff20/Transmission-Dynamics-of-an-Outbreak-of-the-COVID-19-Delta-Variant-B16172-Guangdong-Province-China-May-June-2021.pdf)  

[^6]:
     Campbell F, Archer B, et al. [Increased transmissibility and global spread of SARS-CoV-2 variants of concern as at June 2021](https://www.eurosurveillance.org/content/10.2807/1560-7917.ES.2021.26.24.2100509). Euro Surveillance 2021.

[^7]:
     Centers for Disease Control and Prevention – [SARS-CoV-2 Variant Classifications and Definitions](https://www.cdc.gov/coronavirus/2019-ncov/variants/variant-info.html#Concern). Last Updated September 20, 2021.

[^8]:
     Screenshots courtesy of the German Corona-Warn app team.

[^9]:
     Gantiva C, Jiménez-Leal W and Urriago-Rayo J (2021) Framing Messages to Deal With the COVID-19 Crisis: The Role of Loss/Gain Frames and Content. _Front Psychol_ 12:568212. doi: 10.3389/fpsyg.2021.568212

[^10]:
     Dryhurst S, Schneider CR, Kerr J, Freeman AL, Recchia G, van der Bles AM, et al. (2020). Risk perceptions of COVID-19 around the world. _J Risk Res_ 23, 994–1006. doi: 10.1080/13669877.2020.1758193

[^11]:
     Lohiniva AL, Sane J, Sibenberg K, Puumalainen T,  Salminen M. (2020). Understanding coronavirus disease (COVID-19) risk perceptions among the public to enhance risk communication efforts: a practical approach for outbreaks, Finland, February 2020. _Eurosurveillance_ 25:317.

[^12]:
     Everett JAA, Colombatto CC, Chituc VV, Brady WJJ, Crockett M. (2020). The effectiveness of moral messages on public health behavioral intentions during the COVID-19 pandemic. _PsyArXiv _(in press).

[^13]:
     Barari S, Caria S, Davola A, Falco P, Fetzer T, Fiorin S, et al. (2020). Evaluating COVID-19 public health messaging in Italy: self-reported compliance and growing mental health concerns. _MedRxiv_ 1, 1–19. doi: 10.1101/2020.03.27.20042820

[^14]:
     Falco P, Zaccagni S. (2020). Promoting social distancing in a pandemic: beyond the good intentions. _OSF_. doi: 10.31219/osf.io/a2nys. [Epub ahead of print].
