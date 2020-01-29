# salesforce-account-owner-change
Change owner of Salesforce Account without changing owners of child records

This is a solution to the problem described in Salesforce Idea [Changing Account ownership](https://success.salesforce.com/ideaView?id=08730000000BqqOAAS). When a user presses "Change" next to Owner on Account, child record owners are also transferred to the new owner (opportunities, contacts, and perhaps a few others).

1. Create a new VisualForce page, and paste the following code into it. Name the page AccountChangeOwner. Check the "Available for Lightning Experience, Lightning Communities, and the mobile app" box. Save it. Edit the security settings; enabling it for all users.

```
<apex:page standardController="Account">
    
    <apex:outputPanel id="messages">
        <apex:pageMessages />
     </apex:outputPanel>
     
     <apex:form>
     <apex:pageBlock title="Change owner for account {!Account.Name}">
        <apex:pageBlockButtons > 
            <apex:commandButton action="{!save}" value="Save" />
            <apex:commandButton value="Cancel" 
                action="{!URLFOR($Action.Account.View, Account.Id)}"
                immediate="true"
                title="return to previous page"/>
        </apex:pageBlockButtons> 
             
         <apex:pageBlockSection >
             <apex:inputField value="{!Account.OwnerId}"/>
         </apex:pageBlockSection>
     </apex:pageBlock>
     </apex:form>
</apex:page>
```

2. Create a new formula field on Account. Name it Account Owner, make it of type Text, and give it the following formula:

```
HYPERLINK("/apex/AccountChangeOwner?id=" + Id, 
Owner.FirstName  & " " &  Owner.LastName + " [Change]", 
'_self')
```

