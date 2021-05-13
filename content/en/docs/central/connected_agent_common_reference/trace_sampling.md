---
title: Trace sampling
linkTitle: Trace sampling
draft: false
weight: 10
description: Understand how the Traceability Agent can sample the transaction
  information that is sent to Amplify Central.  Learn how you can control the
  sampling by using the sampling configuration. 
---
## Objectives

Learn how to set up sampling rules used to send only certain transactions to Amplify Central. With the agent default configuration, all transactions are sent to Amplify Central.

## Sampling

Currently, sampling only supports the configuration to send a specific percentage of transactions to Amplify Central.

### Preparing Traceability Agent

The Traceability Agent may be configured to sample fewer than all events.

The following is a sample value that is added to `ta_env_vars.env`, which will send 50% of the gateway transactions to Amplify Central.

```shell
TRACEABILITY_SAMPLING_PERCENTAGE=50
```
