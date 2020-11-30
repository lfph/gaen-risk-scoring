# Configuring Exposure Notification Risk Scores


## Guidance from the Risk Score Symposium Invitational


# Introduction

Exposure notifications are phone-based alerts that people can receive to inform them that they’ve been exposed to someone diagnosed with COVID-19. The Exposure Notification System (ENS) provided by Apple and Google has a configurable component, a risk score, which allows health authorities to specify which types or levels of exposures should trigger a notification.  

In a series of meetings in November 2020, the Risk Score Symposium Invitational brought together specialists and experts from multiple disciplines who were actively involved in ENS implementation and/or research. The symposium’s purpose was to inform the decision-making process for health authorities who are using, or plan to use, ENS in their region.

Below, we provide risk score parameter guidance based on currently available evidence. These recommendations are intended to be shared and open sourced, so the larger community can contribute to and improve upon these recommendations, especially as more data becomes available.

This guidance should be viewed as a starting point; each public health authority will need to make their own decisions tailored to the communities they are serving.


# Structure of the risk score

The risk score estimates, in minutes, the degree of relevant exposure to someone with a COVID-19 diagnosis. It is calculated by multiplying two[^1] weighted terms together: (1) duration at Bluetooth attenuation and (2) infectiousness based on days since symptom onset.  Further documentation on the structure of the risk score is available from [Apple](https://developer.apple.com/documentation/exposurenotification/enexposureconfiguration) and [Google](https://developers.google.com/android/exposure-notifications/meaningful-exposures) technical specification documents.

![Risk score overview](/img/risk-score-overview.png "Risk scores consist of attenuation, infectiousness weight, and (optionally) report type")


_Image source: [https://developer.apple.com/documentation/exposurenotification/enexposureconfiguration](https://developer.apple.com/documentation/exposurenotification/enexposureconfiguration)_


# Configuring Bluetooth attenuations 

Bluetooth attenuations offer a very noisy signal of proximity, impacted by the device hardware, phone cases, environmental surroundings (obstacles and reflections), phone antenna orientation, placement in pockets/bags, how the device is held, and more.  

Based on available data, the signal strength working group considered tradeoffs for capturing particular distance and duration scenarios.  The Signal Strength Working Group agreed on two consensus configuration options:



*   <span style="text-decoration:underline;">Narrower-net specification</span>: the intention is to **prioritize specificity**, favoring sending fewer notifications to capture some fraction of close contacts while limiting the number of further-distance exposures captured 
*   <span style="text-decoration:underline;">Wider-net specification</span>: the intention is to **prioritize sensitivity**, favoring sending more notifications to capture most close-contact exposures and a non-negligible amount of further-distance exposures 

<table>
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
   <td rowspan="2" ><strong>Narrower Net</strong>
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
   <td rowspan="2" ><strong>Wider Net </strong>
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



# Configuring infectiousness 

Timing of exposure relative to symptom onset is [known to impact risk of COVID-19 transmission](https://www.medrxiv.org/content/10.1101/2020.09.04.20188516v2). 

The recommendations below assume a known symptom onset date; no consensus was reached on how to treat cases with no symptom onset date.  For individuals who test positive without symptoms (or do not provide information on symptom onset), using the test date in place of symptom onset date is likely to overestimate transmission risk. Ignoring these individuals would likely underestimate transmission risk.

Based on available data, the clinical/epidemiology working group agreed on two potential consensus configurations:



*   <span style="text-decoration:underline;">Narrower-net specification</span>: the intention is to **prioritize specificity**, favoring sending fewer notifications restricting to exposures during the period of peak infectiousness
*   <span style="text-decoration:underline;">Wider-net specification</span>: the intention is to **prioritize sensitivity**, favoring sending more notifications and capturing exposures over a longer period of potential infectiousness 

<table>
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
   <td>Standard weight
   </td>
   <td>30%
   </td>
   <td>60%
   </td>
  </tr>
  <tr>
   <td>High weight
   </td>
   <td>100%
   </td>
   <td>200%
   </td>
  </tr>
</table>



# 


# Putting it all together


## Narrower-net consensus configuration

![Narrower net](/img/narrower-net.png)


## Wider-net consensus configuration

![Wider net](/img/wider-net.png)


# Choosing notification thresholds

The weights for the configurations above assume health authorities will prefer to use a threshold of 15 minutes (for ease of communication and consistency in messaging).  While this parameter can also be adjusted, the group did not evaluate the impact of changing it.  

The symposium contributors considered messaging associated with notifications to be out of scope in composing this recommendation. Messaging should be crafted by the health authority to be situationally appropriate.  Health authorities may also consider using multiple thresholds to send more targeted text/instructions based on the risk of the encounter.


# Further information and resources

To discuss this document with the working group please join the Linux Foundation Public Health Slack channel at [slack.lfph.io](slack.lfph.io). In that community you will also be able to discuss exposure notification rollouts with other public health authorities who have launched their own solutions. 

The SwissCovid team has released data from their real-life Bluetooth measurements at: [https://github.com/DP-3T/bt-measurements.](https://github.com/DP-3T/bt-measurements)

The experiments evaluated several real-life scenarios and provide both ground truth distance/attenuation data along with raw dumps of GAEN messages that can be used for the analysis of different configurations.

Details about the GAEN API and protocol description are at: [https://www.google.com/covid19/exposurenotifications/](https://www.google.com/covid19/exposurenotifications/)

The DP3T white papers for the underlying exposure notification protocol are available at [https://github.com/DP-3T/documents/](https://github.com/DP-3T/documents/) 

The [COVID Tuner risk score configuration tool](https://risk-score-tuner.appspot.com/) is freely available to health authorities.  Please contact Google developer relations for login information. 


# Contributors

We would like to recognize all participants in the Risk Score Symposium Invitational for their contributions (listed alphabetically):

_Justus Benzler, Robert Koch Institut_

_Mark Briers, United Kingdom Department of Health & The Alan Turing Institute_

_Michael Flowers, New Jersey Office of Innovation_

_Sameer Halai, WeHealth_

_Bronwyn Harris, Apple_

_Bryant Thomas Karras, Washington State Department of Health_

_Emily Kosten, MIT Lincoln Laboratory_

_Randy Mardsen, Apple_

_Joanna Masel, University of Arizona and WeHealth_

_Timothy W Menza, Oregon Health Authority and Oregon Health and Science University_

_Michael McConnell, Google Health_

_Kevin Murphy, Google Research_

_Ravi Palanki, Google_

_Mathias Payer, École Polytechnique Fédérale de Lausanne_

_Rajesh Pillai, University of Alabama at Birmingham _

_Ramesh Raskar, MIT Media Lab and PathCheck Foundation_

_Ronald Rivest, MIT_

_Kathryn Rough, Google Health_

_Jenny Wanger, Linux Foundation Public Health_

_Sam Zimmerman, PathCheck Foundation_

_Marc Zissman, MIT Lincoln Laboratory_



<!-- Footnotes themselves at the bottom. -->
## Notes

[^1]:
     A third weighted term, the exposure report type, is also available but was not discussed during the symposium. Based on current best practices, we assumed only confirmed positive tests would be used.
