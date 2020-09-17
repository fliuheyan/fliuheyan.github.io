## Story

### Expected upfront = no but expected business upfront amount is still calculated

##### Description
https://smartline.zendesk.com/agent/tickets/52528

##### Context:

When a loan is settled, the adviser checks “yes” to expected upfront / expected trail.

If no, then no upfront/trail is expected for the loan. Currently, if no upfront is expected, the expected business upfront amount is still calculated.

We'd need to be sure that this would not affect Connect - it may cause commissions not to be reconciled if someone accidentally flagged upfront as not being expected.

##### Required:

Analysis - will Connect is impacted if we don’t calculate “expected business upfront” when no upfront is expected - This does not impact Connect

Implement change so that when adviser selects no to expected upfront (at settlement) then do not show expected upfront calculated amount

Adviser/Group office commissions team can change “yes/no” to expected upfront; hide/show accordingly AND recalculate on change

In Reports; expected business upfront is shown in Expected settlement graph. If no is selected then this amount shouldn’t be included for the month (Confirm with Graeme about expected behaviour and Xiaofan/HanQuan)


 

##### Questions:

Do we need to do clean up for the existing data ? - effort is big since we need to clean for both loan management DB and big query

No needed for big query

Do we need it for group office reporting (big query) ? 

Sarah has confirmed with Duncan, It doesn’t matter for big query, so no effort here

##### Estimation:

At least 3 points if no data clean up and no impact for big query


### Comments

#### KKK
Based on my understanding ‘expected business upfront’ or ‘expected upfront’ is just a value calculated for the advisers, that is not sent back to Connect.

The flag whether the loan expects and upfront only impacts whether we can reconcile a loan with a trail (in Connect) and also impacts the status of the loan and reports (loan management). It does not have impact on whether we calculate the upfront% or expected upfront or expected business upfront at the moment.

I suppose these flags (upfront expected and trail expected) would also feed into reports tab, so we need to check how the current behaviour affects the reporting (tableau).


#### JJJJ

From what I can see, when we calculated the expectedUpfront or expectedBusinessUpfront, we didn't check for this flag. 
This might be mis-leading to the adviser since if they see this amount and yet they don't expect anything.
Also, I can see that when we process payment, while locking in the amounts, we set these values including, upfrontPercentage (which we send back to connect). We might also need to revisit these values.
Just to be clear, I’m sure in commission calculation, connect will check the whether upfront is expected while calculating this upfront Percentage, so the commission will be correct. However, for someone viewing values in the system, it will be confusing.

This explains connect/LP integrations more.

other refs:
https://git.realestate.com.au/smartline/loans-api/blob/master/src/main/java/com/smartline/loans/api/controllers/LoansController.java#L113

https://git.realestate.com.au/smartline/loans-api/blob/master/src/main/java/com/smartline/loans/api/services/LoanCommissionService.java#L155


### View loan book size, runoff and runoff rate

#### ACs

AC2-View current loan book size section

AC2.1-View “Current loan book size” titleGiven I have commission permissionWhen I am on reports pageThen I can view current loan book size section

AC2.2-View dollar amount of loan book sizeGiven I have commission permissionWhen I am on reports pageThen I can view:-dollar amount of current loan book size -last updated time

AC2.3-View dollar amount of loan book size (last year)to comparisonGiven I have commission permissionWhen I am on reports pageThen I can view:-dollar amount of loan book size (last year)to comparison-Month of last year

AC3- View monthly loan book runoffAC3.1- View monthly loan book run off titleGiven I have commission permissionWhen I am on reports pageThen I can view monthly loan book runoff section

AC3.2-View monthly loan book run offGiven I have commission permissionWhen I am on reports pageThen I can view:-loan book runoff of the current processing month-current processing month-loan book runoff of this month last year to comparison-this month last year

AC4-View yearly loan book run off rateAC4.1- View yearly loan book run off titleGiven I have commission permissionWhen I am on reports pageThen I can view yearly loan book run off rate section

AC4.2-View monthly loan book run offGiven I have commission permissionWhen I am on reports pageThen I can view:-loan book runoff rate rolling 12 month back -time range -loan book runoff date of last year to comparison-time range of last year

AC5-View expected business upfront filed in expected settlement reportGiven I have commission permissionWhen I am on reports pageThen I can view "Expected business upfront" in expected settlement report(tooltip of expected settlement report)


### Manually edit rebates and LMI while doing the product comparison

Marge the card  to this card.Context-Editing the interest rates in product comparison.

Assumption-Edits within the product comparison will be re-usedIn scope-

Allowing for negative fees to cater for bonuses/rebate?

Add LMI field (with no calculation or value) and edit LMI (with a value Adviser might have generated from a lender quote)

Save all the edits made in the comparison

Add upfront fees/rebate and LMI into standalone product search tooAcceptance criteria

Adding LMI Field in product comparison pageGiven I am viewing the product comparison pageWhen one or more products have been selected for comparisonThen I see a new field LMI added to the comparison table(prefix with a '$' symbol)Default state- $0 for LMI fieldGiven I am viewing the comparison pageAnd one or more products have been selected for comparisonThen the default state for the LMI field  is “$0.00”Override behavior for LMI (already built in funding worksheet, re-use that component)Given that the override field are at the default value of “$0.00” When I click on the field to override Then I can see the override modal

Given I can see the override modal When I enter the override value Then the “Apply” button becomes active

Given the “Apply” button is activeWhen I click on it Then the modal closes And the new value is appliedAnd I can see the default value crossed out the new value displayed in its place

Given I see the new value appliedWhen I click on it Then I can see the override modal againAnd the Apply button is inactive

Given I am on the override modal When I clear the value manuallyThen the “Apply” button becomes active

Given I have cleared the value manuallyWhen I press “Apply”Then the modal closes And I can see “$0.00” as the default value

Given I am on the override modal When I click the “x” button or outside the modal Then the modal closes and any changes are not appliedRenaming Upfront fee to Upfront fee/rebateGiven one or more products are selectedAnd I am viewing the product comparisonThen I would see a field called “Upfront fee/rebate“ in the comparison tableUpdating application fee to Application fee/rebateGiven I am viewing the comparisonWhen I click on the Upfront fee/rebateThen I see a modal that has Application fees/rebate listed on the topNote-Modal, fees, entering behavior and apply button already existsAllowing for -ve Application fees/rebateGiven I am on the “Upfront fee/rebate“ modalWhen I enter a negative value(for rebate) in the Application fees/rebateThen the same would be added as a negative fees to the Total upfront fee/rebateAnd I can use ‘Apply’ the changes SaveGiven I have overridden any of the following-Upfront fee/rebate, ongoing fee, LMI, standard interest rate for any of the products in the comparison page When I save the comparisonThen these values will be saved with the comparisonQuestion-Do we need to save both old and updated value ? Unsaved changesImplement the unsaved changes modal as implemented in other places.

Other ususals:1. LIXI/AOL mapping2. Permissions3. Activity log4. Toggle5. Responsiveness6. Accessibility  1. to confirm if all the above changes needs to go into standalone product search?-Only LMI and Upfront fee/rebate should go in standalone.2. Do not allow for negatives in LMI. Also check in AOL3. This LMI would go into AOL


#### Comments

##### K
questions:

“Edit LMI“ little harder to implement. Hardcoded value is “Edit field“. Shell we go with default or cater for future modals with custom header?

“Total upfront fee” stays a “fee” even if it’s negative. Should we rename it as well?

To Jafari Sitakange . what is the correct LMI container in application-api? - I’m assuming VariantsCombination

##### A
Why would we allow for negative fees? This is not something we’ve allowed so far. - advisers can have discounts up to negative numbers from the lenders

Do we need to audit the overrides made by the advisers on the comparison page? (Upfront fee, Ongoing fee, Standard interest rate, LMI) We also added the LMI field to be entered on comparison.-Yes it should be.

the styling of the fields that can be overridden should be changes in the read only mode - show black instead of blue for Upfront fee, Ongoing fee, Standard interest rate (check how it’s done on the Funding worksheet tab).-Note for Shiraz Visinko and dev’s

we need to store the overrides and LMI as part of the comparison - currently overrides are not stored as part of comparison.-Yes

need to clarify what we want for AOL/LIXI mapping - need to be in actual ACs as currently it’s just too general.-Note for aakriti.narang 

##### K
Kick-off notes: Issy Li Ilya Khromov 

need to update the total upfront label (talk with Shiraz Visinko )

this should have a separate toggle for standalone product search as we might release it earlier than for embedded product search and don’t want to reveal unfinished work in the standalone product search

we don't need to have the cross-out for LMI as we are not overriding anything

need to answer to the question whether we want to record the overrides in the audit as e.g. we are not recording the extra features in there at the moment (only record the comment and selected product). aakriti.narang Shiraz Visinko 


##### LL

need to update the total upfront label

update “upfront fee”, “application fee” and “total upfront fee” to be “xxx/rebate”

we don't need to have the cross-out for LMI as we are not overriding anything

keep the cross out, so it will always be $0 to new value.

need to answer to the question whether we want to record the overrides in the audit as e.g. we are not recording the extra features in there at the moment (only record the comment and selected product). aakriti.narang Shiraz Visinko

We do need to record it, please come up with the fields that we need to record.

Use same design in product comparison editing modal instead of the funding worksheet one.
