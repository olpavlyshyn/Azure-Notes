## Indentity
- AAD Object Types
    - User
    - Groups (Assigned, Dynamic)
    - Enterprise Apps, Azure Resorces (Service Principals)
    - Managed Identities
    - Devices
    - Stuff
- Authentication
    - for AAD there is cloud and federal authentication
        - Password Hash (Cloud)
        - Pass-Through Authentication (PTA) (hybrid)
        - Federation (hybrid)
    - PTA and Federation has benefits releted to locked accounts, logon hours, expired passwords
- Privilaged Identity Management
    - Enables Elevation of AAD (and ARM) when neede for limited time
    - Roles must be pre-assigned to be available for users
    - Users than elevate on demand or for a futer time period
    - AAD P2 feature
- AAD MFA
    - AAD P1 or use Security Default (or be global administrator)
    - 3-Party MFA can be used
- Conditional Access
    - Is triggered for any authorization regadless of authentication method
    - AAD P1 +
- B2B Guests
- B2C
    - is aimed to our customers as a separate type and tenant instance that is fully castomizable with other types of social identity support
- Azure AD is identity provider


## Azure Active Direcory

* Security Principals:
    - A security principal is an object in AAD
    - It can represent:
        - User
        - Group
        - Service principal
        - Managed indentity
    - These can be cloud accounts, replicated from AD or guests (B2B)
    - The security principal is used to authn and is authzed for certain access and actions as part of authz
- RBAC will override any ACLs on object within the RBAC scope