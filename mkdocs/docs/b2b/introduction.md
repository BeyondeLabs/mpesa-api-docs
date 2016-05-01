> **Source:** M-PESA / Developer Guide - B2B Interface, v 0.3 (April 24, 2015) (c) Safaricom

## Scope
The present document specifies the real time B2B Web Service aspects of the interface. All aspects of B2B Web Service are defined here, these being:

- Message Flow Description
- DataTypeDefinition
- Web Service Interface Definition
- TransactionTypesdescription
- WSDL for this specification
- Example

## Overview - API

This document details the interface specification for a real time M-Pesa transaction API delivered within the MPesa Core API Framework. The Core API defines an abstract model for API requests which includes 4 distinct parties to API operations, as well as a generic set of API operation parameters.

`Initiators` are the organization or entity requesting that an API operation or transaction is performed. 

The `Caller` is the actual piece of software which communicates with the MPesa Core API web service. 

The `Primary Party` is (typically) the debit party within an existing MPesa financial transaction.

The `Receiver Party` is correspondingly the credit party in an MPesa financial transaction.

