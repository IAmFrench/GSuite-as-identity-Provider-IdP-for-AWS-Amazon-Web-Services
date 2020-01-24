# GSuite-as-identity-Provider-IdP-for-AWS-Amazon-Web-Services
_Use GSuite accounts for AWS_

## Google Admin pre-requirements

### Add custom user attributes (for AWS SAML binding attributes)
_[Gsuite Admin Help](https://support.google.com/a/answer/6208725?hl=en)_

_[AWS Docs](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_saml_assertions.html)_

> [GSuite Admin > Users > Manage user attributes](https://admin.google.com/ac/customschema)

1. Add AWS SAML Role Attribute (Text, Multi-value)
2. (Optional) Add AWS SAML RoleSessionName Attribute (Text, Single Value)

![AWS SAML Custom Attributes](https://imgur.com/L9MolFj.png)


## Set up SAML app (choose Amazon-Web-Services)

### Create a new SAML App (choose Amazon-Web-Services)

> [GSuite Admin > Apps > SAML Apps](https://admin.google.com/AdminHome?fral=1#AppsList:serviceType=SAML_APPS)

![GSuite SAML Apps](https://imgur.com/qnhurpu.png)

### Download IDP Metadata (XML)
![Download IDP Metadata](https://i.imgur.com/TWNjNV2.png)

### Edit Service Provider Details
> Name ID = Basic Information > Primary Email    
> Name ID Format = Email    

![Edit Service Provider Details](https://i.imgur.com/EuwpgUk.png)

### Set Attribute Mapping ([AWS Docs](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_saml_assertions.html))

> `https://aws.amazon.com/SAML/Attributes/RoleSessionName`    
> `https://aws.amazon.com/SAML/Attributes/Role`    
> `https://aws.amazon.com/SAML/Attributes/SessionDuration`    

![Set Attribute Mapping - RoleSessionName](https://i.imgur.com/65SJbo6.png)
![Set Attribute Mapping - Role](https://i.imgur.com/1RBMLZf.png)

![Create SAML App Done](https://i.imgur.com/z6RTfSV.png)

## Config AWS IAM Provider

### Create the GSuite Provider in AWS Console
> [AWS > IAM > Access management > Identity providers](https://console.aws.amazon.com/iam/home#/providers)

![AWS > IAM > Access management > Identity providers List](https://i.imgur.com/Jcyb5c3.png)

### Configure Provider
> Set a friendly name    
> Set provider type to SAML    
> Select the Gsuite IDP Metadata (XML file)    

![Configure Provider > SAML](https://i.imgur.com/6o0HVm1.png)

![Create the GSuite Provider Done](https://i.imgur.com/eIwvYlX.png)

## Configure IAM Role(s) in AWS Console
_For this example we will create 2 roles, 1 administrator and 1 Read only role_    
_If an user has 2+ assigned roles, he can chose the desired role_    
> [AWS > IAM > Access management > Roles](https://console.aws.amazon.com/iam/home#/roles)    

![AWS > IAM > Access management > Roles](https://i.imgur.com/L7PouEQ.png)

### Create SAML Role (Administrator)
> [AWS > IAM > Access management > Roles > Create role](https://console.aws.amazon.com/iam/home#/roles$new?step=type&roleType=saml)    

![Create SAML Role](https://i.imgur.com/kAnD8Pm.png)

> Select AdministratorAccess policy    
> Name: Administrator    
![Create SAML Role - Administrator](https://i.imgur.com/ziICWgY.png)

### Create SAML Role (ViewOnlyAccess)
> Do the same as for Administrator but with the ViewOnlyAccess policy    
> Name: ViewOnlyAccess    
![Create SAML Role - ViewOnlyAccess](https://i.imgur.com/bHfugfF.png)

![Create roles Done](https://i.imgur.com/tfaJ2cF.png)

## Configure GSuite role (user attribute)
_[AWS Doc](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_saml_assertions.html)_    

### Find Role(s) ARN
![Role ARN](https://i.imgur.com/iz0lufl.png)

### Find GSuite Identity provider ARN
![GSuite Identity provider](https://i.imgur.com/1owqlZK.png)

### Set GSuite user attribute
_Format of the AWS SAML Role Attribute:_    
_arn:aws:iam::account-number:role/role-name1,arn:aws:iam::account-number:saml-provider/provider-name_    
_SAML SessionDuration Attribute in seconds_    
![User > configure attribute > AWS SAML attributes](https://i.imgur.com/ueElNNy.png)

## Check the set-up

### Open Google & launch the AWS Quick link
![Launch the AWS GSuite link](https://i.imgur.com/nygLGHN.png)

### Select the AWS Role (if you have assigned multiples roles to a GSuite user)
![Select the AWS Role](https://i.imgur.com/Yv9VOfh.png)

### Check AWS user details
![AWS User details](https://i.imgur.com/FVPXsO7.png)

## Next steps
- [ ] Create GSuite Groups and auto assign roles based on the user's groups

