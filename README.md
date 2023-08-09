# agent-protocol

Snow Mountain AI's agent protocol defines an interface for AI-powered agents and traditional computing systems to collaborate on complex projects. It has been designed to be simple and flexible, allowing long-running agents to be used either as tools by other agents or steps in larger workflows. 

This is inspired from and is compatible with [e2b's agent protocol](https://github.com/e2b-dev/agent-protocol/)..

### Request

```
POST https://agentprovider.com/agent
Authorization: Bearer {jwt}

{
    input: "Redesign our landing page. Use logo.jpg as the header image.",
    workspace: "git@github.com:org/website.git", 
    callback_url: "https://oursite.com/callback",
}
```

### Response

```
{
    "task_id": "12345",
    "input": "Redesign our landing page. Use logo.jpg as the header image."
    "status_url": "https://agentprovider.com/agent/12345"
}
```

## Task status check

### Request
```
GET {status_url}
Authorization: Bearer {jwt}
```

### Response
```
{
    "status": "in_progress",
    "message": "Cloned the git repo. Now planning the redesign."
}
```

## Callback

The agent may call the callback URL specified in the original request to ask clarifying questions or report task progress (to avoid polling).

### Request
```
POST {callback_url}

{
    "task_id": "12345",
    "question": "Do we need to support mobile view for the new landing page design?",
    "status": "blocked"
}
```


# TODO

- Define status values and possible transitions
- Evaluate multiple workspaces
- Attach files without a workspace (eg: "upscale this image")
- Deterministic step transition