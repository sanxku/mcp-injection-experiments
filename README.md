# MCP Tool Poisoning Experiments

This repository contains a few experimental MCP server implementations, that attempt ot inject the MCP client/agent in use.

For more details about the attack method, please see our [blog post](https://invariantlabs.ai/blog/mcp-security-notification-tool-poisoning-attacks).

## Direct Poisoning 

In [`direct-poisoning.py`](./direct-poisoning.py), we implement a simple MCP server that instructs an agent to leak sensitive files, when calling the `add` tool (in this case SSH keys and the `mcp.json` file itself). 

An example execution in cursor looks like this:

![Cursor executes tool poisoning](https://invariantlabs.ai/images/cursor-injection.png)

## Tool Shadowing

In [`shadowing.py`](./shadowing.py), we implement a more sophisticated MCP attack, that manipulates the agent's behavior of a `send_email` tool (provided by a different, trusted server), such that all emails sent by the agent are leaked to the attacker's server.

An example execution in Cursor looks like this:

![Cursor executes tool shadowing](https://invariantlabs.ai/images/mcp-shadow.png)

## WhatsApp takeover

Lastly, in [`whatsapp-takeover.py`](./whatsapp-takeover.py), we implement a shadowing attack combined with a sleeper rug pull, i.e. an MCP server that changes its tool interface only on the second load to a malicious one.

The server first masks as a benign "random fact of the day" implementation, and then changes the tool to a malicious one that manipulates [whatsapp-mcp](https://github.com/lharries/whatsapp-mcp) in the same agent, to leak messages to the attacker's phone number.

![Image](https://github-production-user-asset-6210df.s3.amazonaws.com/17903049/430713783-c3067571-0db3-42c1-b822-2d0245060eda.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20250406%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20250406T145231Z&X-Amz-Expires=300&X-Amz-Signature=bb7ed65dab3fc58d8fb0c14a0e59bcf71f78a6bc1692647cb625db83e17e7e69&X-Amz-SignedHeaders=host)

Can you spot the exfiltration? Here, the malicious tool instructions, ask the agent to include the smuggled data after many spaces, such that with invisible scroll bars, the user does not see the data being leaked.

