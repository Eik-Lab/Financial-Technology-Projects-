How to Integrate Climate Risk into Credit Risk Assessments
===============================================

##### *Use NFIP insurance data to re-assess mortgage portfolio exposures*

---

_Author: Claire Zhang_

_Source: Towards Data Science_  

_Publication Date: Jul 8, 2021_

_Link: https://towardsdatascience.com/how-to-integrate-climate-risk-into-credit-risk-assessments-f7eac5c01850_

---


A decade ago, climate risk was only viewed as a potential threat to a company’s ethics and reputations. Today more and more companies start considering it as a financial risks that could affect its strategy and business planning. As outlined in the Task Force on Climate Related Financial Disclosure (TCFD) recommendations, there are 2 main categories of climate risks that can cause financial implications to a company: physical risk and transition risk.

**Physical Risks**: _reduced property value or increased operational costs (e.g. higher insurance premium) caused by either the frequency or severity of extreme weather events (i.e. acute risks), such as cyclones, hurricanes or floods; or long-term climate pattern shifts (i.e. chronic risks), such as precipitation, rising temperatures and rising sea levels._

**Transition Risk**: _changes in policy, technology, consumer behavior and market supply associated with the transition to a low-carbon economy, including write-offs or early retirement of fossil fuel reserves or pipelines (i.e. stranded assets) due to sudden change in emission mitigation policies._

According to TCFD’s latest Status Report in 2020, there are over 1,700 companies signed up to disclose climate-related risks and opportunities through its major financial reporting. More importantly, the abundance of climate-related risk data is expected to be largely supplemented by data providers and NGOs, such as [CDP](https://www.cdp.net/en), [2Degree Investing](https://2degrees-investing.org/), and [Carbon4 Finance](http://www.carbon4finance.com/) etc.. It is expected that in order to fully incorporate climate risk into (credit) investment/business decisions, there will be **an increasing demand to explore nonconventional modeling techniques to “translate” the climate scenarios, emission reduction pathway, or carbon management ESG indicators into financial implications**.

In this article, we will discuss how to leverage National Flood Insurance Program (NFIP) claims data to draw insights on the impact of flood risk on property value and thus reveal the hidden risks yet to be captured in the residential mortgage exposures (i.e. building a linkage between property value and expected loss of mortgage portfolios). Let’s get started.

**What is NFIP Program?**
=========================

Flood damage is excluded under standard homeowners and renters insurance policies. However, flood coverage is available as a separate policy from NFIP, a Federal program created by Congress in 1968 and administered by the Federal Emergency Management Agency (FEMA). There are 2 primary goals of NFIP program:

*   To protect property owners in participating communities against losses from flooding;
*   To reduce future flood damages through the adoption of floodplain management regulations by communities and states.

Because of these 2 main purposes, flood insurance is mandatory for building located in a participating NFIP community and in Special Flood Hazard Area (SFHA, a.k.a. 100-year-flood-hazard-zone, since these areas have an annual probability of 1% being hit by flood events)¹, while some mortgage lenders may require flood insurance coverage for properties outside the SFHA as part of their approval process. Furthermore, participation in NFIP program is on a community basis rather than on an individual basis, since the insurance is available only in those flood-prone areas where adequate design and construction standards have been adopted under its community oversight.

NFIP provides 2 kinds of insurance coverages, i.e. Building and Contents coverage under 2 types of programs respectively, i.e. Regular and Emergency Program. Take single-family house as example, NFIP insurances can provide Building Coverage upto $250,000 under Regular program, and $35,000 under Emergency program; with additional Contents Coverage upto $100,000 and $10,000 respectively. **Historically, the highest NFIP payouts belong to Hurricane Katrina in 2005 at 16.3 billion USD, followed by Hurricane Harvey in 2017, at 8.9 billion USD**.

How Flood Event Can Affect Your Mortgage Book Behavior?
=======================================================

In the area damaged or destroyed by flood events, one would expect to see an instant spike in delinquency rates. That is because some homeowners might have to repair significant damages or even rebuild their properties while paying rent for short-term housing, when forbearance program or modifications are not yet offered to them. The figure below demonstrates the instant hike in delinquency rates in Texas after Hurricane Harvey in Aug 2017.

![image](https://github.com/Eik-Lab/NBIM-hackathon/assets/45490612/3d6c76b8-1e8f-46f7-ac3e-9223ce3a3c77)

Also as shown in the figure above, after the financial assistance programs (i.e. forbearance, repayment plans an/or loan modifications) are made available to borrowers, a great amount of delinquent loans will be resolved before rolling into severe delinquency or default status. **The chance of loans became severely delinquent or default are mainly determined by the severity of flood damage (or remaining home equity size), and income status of the borrower**. According to the [case study of Hurricane Harvey](https://www.tandfonline.com/doi/full/10.1080/10527001.2020.1840131) based on Fannie Mae data, compared with properties with no damage, moderate to severe damage (i.e. greater than 10% damage over property value) increases the odds of loans becoming 90 days delinquent during the five months post-flood period by three times.

**Prepayment behavior also surges after the flood events, especially in the areas where flood insurance is required.** This is because these homeowners can use the insurance claims to pay off the outstanding loans. Additionally, it is not surprised that some of the affected homeowners may want to move outside of the flood-prone areas, among which these insured borrowers are more willing to sell the house in a discounted value to the investors since their losses are already covered. This is supported by the [37.2% increase](https://www.wsj.com/articles/how-harvey-transformed-house-hunting-in-houston-1540393824) in house sale activities one year after Harvey hit.

It is evident that in order to describe the heightened loss and prepayment behavior in the post-flood period, the missing link in the traditional credit risk analysis is the use of insurance policies/claims data to answer the following key questions:

1.  How to predict property damage caused by flood events?
2.  How to predict the insurance coverage change in a loan book over time?

Our focus in the below analysis will be on the first question.

Data Download
=============

FEMA provides public access to data sets, including Emergency Management, Hazard Mitigation and NIFP etc., through [OpenFEMA](https://www.fema.gov/about/reports-and-data/openfema), a one stop shop for data delivery via direct downloads (csv format) or API endpoints. The datasets used in this analysis is sourced from [FIMA NFIP Redacted Claims — v1](https://www.fema.gov/openfema-data-page/fima-nfip-redacted-claims-v1), a data set represents more than 2,000,000 claims transactions since 1970s, covering all states and territories. In this analysis, we use the ratio of insurance claims over coverage as a measure of damage, and our focus will be on the single-family houses.

Depth-Damage Relationship
=========================

Since flood depth is the best proxy to estimate property damage, for decades, a conventional way of estimating economic losses caused by flood events is based on depth-damage function. The shape of the depth-damage function is largely determined by the following property characteristics:

> Number of Floors

It appears that the higher the number of floors (herein Townhouse represents townhouse with three or more floors), the lower the exposure to flood damages. The highest risk belong to Manufactured (mobile) home or travel trailer on foundation.

![image](https://github.com/Eik-Lab/NBIM-hackathon/assets/45490612/f8dd0588-9819-4b02-adcc-8c78173a8bcb)

> Basement Types

Single-family house with no basement (including basement type — unknown) tends to show higher risks of damages as compared to the ones with basement structure. As expected, when a basement is more complete and similar to its upstairs living areas, the property tends to show lower risk of flood damages.

![image](https://github.com/Eik-Lab/NBIM-hackathon/assets/45490612/ec6bd95b-18b0-4089-a498-8176bc6e5948)

> Flood Zone (Flood Insurance Rating Map — FIRM)

FEMA assigns and maintains flood zone rating to help mortgage lenders determine insurance requirements and help communities develop strategies for reducing their risks. Flood zone is defined based on flood risk drivers such as probability of flood hits, area characteristics (e.g. coastal, ponding etc.), construction standards and etc.. Refer to table below for details.

![image](https://github.com/Eik-Lab/NBIM-hackathon/assets/45490612/588253b5-ded0-43c2-8581-22bea363df31)


_Original Source: [https://floodpartners.com/flood-zones/](https://floodpartners.com/flood-zones/)_

It is observant that damage ratio is fairly sensitive to the flood zones, however not necessarily the highest damage ratios are always associated with the high risk flood zones. In some cases, it is quite the opposite where low to moderate risk zone such as Zone C and Zone X have shown high damage ratios as well. This is probably because the static flood zone rating introduced by FEMA since 1970s can no longer provide accurate measurement of the area’s intrinsic flood risks. In fact, earlier this year, FEMA has announced to launch **Risk Rating 2.0** in Oct 2021. The new risk rating methodology is expected to incorporate more flood risk variables, including flood frequency, multiple flood types — river overflow, storm surge, coastal erosion and heavy rainfall — and distance to a water source along with property characteristics such as elevation and the cost to rebuild.

![image](https://github.com/Eik-Lab/NBIM-hackathon/assets/45490612/4eb92d79-b8d9-417e-a034-8cee20d86f4a)

Similar conclusion can be drawn from figures below, which present an alternative view on the damage ratio caused by Hurricane Harvey and its comparison to the SFHA zone designation of the same area.

![image](https://github.com/Eik-Lab/NBIM-hackathon/assets/45490612/69edf462-70be-47bb-b6f6-d6173479d5bb)

When placing the above two figures on top of each other, one can see from the figure below that many areas subject to high damages (blue shades) are actually located outside of SFHA zone.

![image](https://github.com/Eik-Lab/NBIM-hackathon/assets/45490612/210d2966-9f6a-4343-9bdb-6fc48254148b)

What’s Next?
============

The empirical analysis above tries to demonstrate the intuitiveness and importance of using insurance data in climate-related credit risk analysis. Outlined in the beginning of this article, as a next step, one may want to explore the depth-damage function to generate property-level annual loss curve under various climate scenarios. FEMA’s [**Hazus Flood Assessment Structure Tool (FAST) Tool**](https://github.com/nhrap-hazus/FAST), an open source solution, can be a great starting point for you. Although over the years many hydrologists have raised concerns on the hazard data sparsity limitation and criticized about applying Hazus model to all regions and flood events, it remains the most popular foundations for various climate analytics vendor solution when it comes to predicting hazard damages.

Another step to be considered is to predict the insurance coverage evolution in long-term climate scenario analysis. Apart from historical trend, modeling insurance coverage should consider 1) the exposure of underestimated flood risks; and 2) the adverse impact of increasing insurance pricing on low income neighborhoods.

Conclusion
==========

Hope this article helps you think about how to incorporate insurance data into your credit risk analysis and quantify the impact of climate risks. Leave your comments below if you like it. The full code of the above analysis (including the Basemap zip code level plot) can be found [**here**](https://gist.github.com/domisoxz/be99af5b3863e041b7cb793097db2e12).

_Use Basemap to plot zip code level damage ratio & flood zone rating for Hurricane Harvey (Houston, TX)_

**Footnote**

[1] SFHA: SFHA areas have at least one-in-four chance of flooding during a 30-year mortgage. If one would like to know whether or not his or her property is in a SFHA zone, you can locate this information using your address or coordinates on [https://msc.fema.gov/portal/search](https://msc.fema.gov/portal/search). Leave your comment below, if you would like to know further about how to extract SFHA flood zone information through FEMA’s NFHL (National Flood Hazard Layer) API.
