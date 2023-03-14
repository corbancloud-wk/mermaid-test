# Sequence Diagram

```mermaid
sequenceDiagram
    autonumber
    
    actor U as User
    participant FE as FrontEnd

    
    U->>FE: Request Home Page
    critical Initial Authentication and Authorization
        FE->>+frontend_web_service: GET /manifest
        frontend_web_service->>-FE: return 200 OK 

        FE->>+iam_service: GET /security/authentication
        iam_service->>-FE: 200 OK
        
        FE->>+identity: GET /abilities
        identity-->-FE: 200 OK
    end

    FE->>licensing_api: /v4/user 'getContext'
    licensing_api->>FE:response

    FE->>licensing_api: /v4/user 'getSolutionTypesByLicenseFamily'
    licensing_api->>FE:response

    FE->>licensing_api: /v4/user 'getAbilities'
    licensing_api->>FE:response


    Note right of FE: User metadata requests made concurrently
    par Retrieve user metadata
        FE->>view_settings_service: getUserSettings
        view_settings_service->>FE:response
    and 
        FE->>workspaces_v4: getWorkspace
        workspaces_v4->>FE:response
    and 
        FE->>workspaces_v4: getOrganizationMembershipsForUser
        workspaces_v4->>FE:response
    and
        FE->>licensing_api: getSolutionTypesByLicenseFamily
        licensing_api->>FE:response
    end


    Note right of FE: Launchpad Really Starts Down Here


```

## Mermaid Notes

- VSCode preview works on .md files only so create a .md w/ embedded mermaid instead of .mmd file
- Mermaid doesn't seem to have grouping for participants

# Launch Pad Sequence

## Questions
- Frugal subscriptions seem to correlate to a back end? Sub#2 is consitently used
for licensing API for example
- How are subcription IDs(?) formed, looks like `workspace + JWT + ???` 
mostly like `QWNjb3VudB8xNTMwODAwMjAw.TWVtYmVyc2hpcB8xNTE0ODYwMzQ4.243159c1-7979-4744-82e0-07a413121133`
but also `QWNjb3VudB8xNTMwODAwMjAw.TWVtYmVyc2hpcB8xNTE0ODYwMzQ4.licensing-service.Events.EventCreated`
and also
`organization_rule.3730edc2-7091-4eaf-ab1c-513f44fabd3f.V0ZVc2VyHzQ2MTIzODIzODA1MjM1MjA.workspaces.OrganizationUser.WorkspaceMembershipRemoved`

ConfigUI team (support-landingpage)
