title: Offline signatures

This section explains how to authorize transactions with private keys that are kept **offline**. In particular, this guide shows how to create and save transactions to a file that can then be transferred to an offline device for signing. To learn about the structure of transactions and how to authorize them in general visit the [Transactions Structure](../../transactions/) and [Authorizing Transactions](../../transactions/signatures/) sections, respectively.

The same methodology described here can also be used to work with [LogicSignatures](../../transactions/signatures/#logic-signatures) and [Multisignatures](../../transactions/signatures/#multisignatures). All objects in the following examples use msgpack to store the transaction object ensuring interoperability with the SDKs and `goal`.

!!! info
    Storing keys _offline_ is also referred to as placing them in **cold storage**. An _online_ device that stores private keys is often referred to as a **hot wallet**.  

# Unsigned Transaction File Operations
Algorand SDK's and `goal` support writing and reading both signed and unsigned transactions to a file. Examples of these scenarios are shown in the following code snippets.

Unsigned transactions require the transaction object to be created before writing to a file.

=== "JavaScript"
	<!-- ===JSSDK_CODEC_TRANSACTION_UNSIGNED=== -->
	```javascript
	const txn = algosdk.makePaymentTxnWithSuggestedParamsFromObject({
	  from: sender.addr,
	  to: receiver.addr,
	  amount: 1e6,
	  suggestedParams,
	});
	
	const txnBytes = txn.toByte();
	const txnB64 = Buffer.from(txnBytes).toString('base64');
	const restoredTxn = algosdk.decodeUnsignedTransaction(
	  Buffer.from(txnB64, 'base64')
	);
	console.log(restoredTxn);
	```
	[Snippet Source](https://github.com/joe-p/js-algorand-sdk/blob/doc-examples/examples/encoding.ts#L37-L50)
	<!-- ===JSSDK_CODEC_TRANSACTION_UNSIGNED=== -->

=== "Python"
	<!-- ===PYSDK_CODEC_TRANSACTION_UNSIGNED=== -->
	```python
	sp = algod_client.suggested_params()
	pay_txn = transaction.PaymentTxn(acct.address, sp, acct.address, 10000)
	
	# Write message packed transaction to disk
	with open("pay.txn", "w") as f:
	    f.write(encoding.msgpack_encode(pay_txn))
	
	# Read message packed transaction and decode it to a Transaction object
	with open("pay.txn", "r") as f:
	    recovered_txn = encoding.msgpack_decode(f.read())
	
	print(recovered_txn.dictify())
	```
	[Snippet Source](https://github.com/barnjamin/py-algorand-sdk/blob/doc-examples/_examples/codec.py#L31-L43)
	<!-- ===PYSDK_CODEC_TRANSACTION_UNSIGNED=== -->

=== "Java"
	<!-- ===JAVASDK_CODEC_TRANSACTION_UNSIGNED=== -->
	```java
	Response<TransactionParametersResponse> rsp = algodClient.TransactionParams().execute();
	TransactionParametersResponse sp = rsp.body();
	// Wipe the `reserve` address through an AssetConfigTransaction
	Transaction ptxn = Transaction.PaymentTransactionBuilder().suggestedParams(sp)
	        .sender(acct.getAddress()).receiver(acct.getAddress()).amount(100).build();
	
	byte[] encodedTxn = Encoder.encodeToMsgPack(ptxn);
	
	Transaction decodedTxn = Encoder.decodeFromMsgPack(encodedTxn, Transaction.class);
	assert decodedTxn.equals(ptxn);
	```
	[Snippet Source](https://github.com/barnjamin/java-algorand-sdk/blob/examples/examples/src/main/java/com/algorand/examples/CodecExamples.java#L48-L58)
	<!-- ===JAVASDK_CODEC_TRANSACTION_UNSIGNED=== -->

=== "Go"
	<!-- ===GOSDK_CODEC_TRANSACTION_UNSIGNED=== -->
	```go
	// Error handling omitted for brevity
	sp, _ := algodClient.SuggestedParams().Do(context.Background())
	ptxn, _ := transaction.MakePaymentTxn(
		acct1.Address.String(), acct1.Address.String(), 10000, nil, "", sp,
	)
	
	// Encode the txn as bytes,
	// if sending over the wire (like to a frontend) it should also be b64 encoded
	encodedTxn := msgpack.Encode(ptxn)
	os.WriteFile("pay.txn", encodedTxn, 0655)
	
	var recoveredPayTxn = types.Transaction{}
	
	msgpack.Decode(encodedTxn, &recoveredPayTxn)
	log.Printf("%+v", recoveredPayTxn)
	```
	[Snippet Source](https://github.com/barnjamin/go-algorand-sdk/blob/examples/_examples/codec.go#L25-L40)
	<!-- ===GOSDK_CODEC_TRANSACTION_UNSIGNED=== -->

=== "goal"
	<!-- ===GOAL_CODEC_TRANSACTION_UNSIGNED=== -->
    ``` goal
    $ goal clerk send --from=<my-account> --to=GD64YIY3TWGDMCNPP553DZPPR6LDUSFQOIJVFDPPXWEG3FVOJCCDBBHU5A --fee=1000 --amount=1000000 --out="unsigned.txn"

    $ goal clerk sign --infile unsigned.txn --outfile signed.txn

    $ goal clerk rawsend --filename signed.txn

    ```
	<!-- ===GOAL_CODEC_TRANSACTION_UNSIGNED=== -->
# Signed Transaction File Operations 
Signed Transactions are similar, but require an account to sign the transaction before writing it to a file.

=== "JavaScript"
	<!-- ===JSSDK_CODEC_TRANSACTION_SIGNED=== -->
	```javascript
	const signedTxn = txn.signTxn(sender.privateKey);
	const signedB64Txn = Buffer.from(signedTxn).toString('base64');
	const restoredSignedTxn = algosdk.decodeSignedTransaction(
	  Buffer.from(signedB64Txn, 'base64')
	);
	console.log(restoredSignedTxn);
	```
	[Snippet Source](https://github.com/joe-p/js-algorand-sdk/blob/doc-examples/examples/encoding.ts#L53-L59)
	<!-- ===JSSDK_CODEC_TRANSACTION_SIGNED=== -->

=== "Python"
	<!-- ===PYSDK_CODEC_TRANSACTION_SIGNED=== -->
	```python
	# Sign transaction
	spay_txn = pay_txn.sign(acct.private_key)
	# write message packed signed transaction to disk
	with open("signed_pay.txn", "w") as f:
	    f.write(encoding.msgpack_encode(spay_txn))
	
	# read message packed signed transaction into a SignedTransaction object
	with open("signed_pay.txn", "r") as f:
	    recovered_signed_txn = encoding.msgpack_decode(f.read())
	
	print(recovered_signed_txn.dictify())
	```
	[Snippet Source](https://github.com/barnjamin/py-algorand-sdk/blob/doc-examples/_examples/codec.py#L48-L59)
	<!-- ===PYSDK_CODEC_TRANSACTION_SIGNED=== -->

=== "Java"
	<!-- ===JAVASDK_CODEC_TRANSACTION_SIGNED=== -->
	```java
	SignedTransaction signedTxn = acct.signTransaction(ptxn);
	byte[] encodedSignedTxn = Encoder.encodeToMsgPack(signedTxn);
	
	SignedTransaction decodedSignedTransaction = Encoder.decodeFromMsgPack(encodedSignedTxn,
	        SignedTransaction.class);
	assert decodedSignedTransaction.equals(signedTxn);
	```
	[Snippet Source](https://github.com/barnjamin/java-algorand-sdk/blob/examples/examples/src/main/java/com/algorand/examples/CodecExamples.java#L61-L67)
	<!-- ===JAVASDK_CODEC_TRANSACTION_SIGNED=== -->

=== "Go"
	<!-- ===GOSDK_CODEC_TRANSACTION_SIGNED=== -->
	```go
	// Assuming we already have a pay transaction `ptxn`
	
	// Sign the transaction
	_, signedTxn, err := crypto.SignTransaction(acct1.PrivateKey, ptxn)
	if err != nil {
		log.Fatalf("failed to sign transaction: %s", err)
	}
	
	// Save the signed transaction to file
	os.WriteFile("pay.stxn", signedTxn, 0644)
	
	signedPayTxn := types.SignedTxn{}
	err = msgpack.Decode(signedTxn, &signedPayTxn)
	if err != nil {
		log.Fatalf("failed to decode signed transaction: %s", err)
	}
	```
	[Snippet Source](https://github.com/barnjamin/go-algorand-sdk/blob/examples/_examples/codec.go#L43-L59)
	<!-- ===GOSDK_CODEC_TRANSACTION_SIGNED=== -->

=== "goal"
	<!-- ===GOAL_CODEC_TRANSACTION_SIGNED=== -->
    ``` goal
    $ goal clerk rawsend --filename signed.txn
    ```
	<!-- ===GOAL_CODEC_TRANSACTION_SIGNED=== -->

!!! info
    Example transaction code snippets are provided throughout this page. Full running code transaction examples as well as **offline multisig** for each SDK are available within the GitHub repo at [/examples/offline](https://github.com/algorand/docs/tree/master/examples/offline) and for [download](https://github.com/algorand/docs/blob/master/examples/offine/offline.zip?raw=true) (.zip).
    
??? example "Saving Signed and Unsigned Multisig Transactions to a File using goal"
    
    
    Create a multisig account by listing all of the accounts in the multisig and specifying the threshold number of accounts to sign with the -T flag

    ```bash
    goal account multisig new <my-account1> <my-account2> <my-account3> etc… -T 2    
    ```

    Create an unsigned transaction and write to file

    ```bash
    goal clerk send --from <my-multisig-account>  --to AZLR2XP4O2WFHLX6TX7AZVY23HLVLG3K5K3FRIKIYDOYN6ISIF54SA4RNY --fee=1000 --amount=1000000 --out="unsigned.txn"
    ```

    Sign by the required number of accounts to meet the threshold. 

    ```bash
    goal clerk multisig sign -a F <my-account1> -t=unsigned.txn
    goal clerk multisig sign -a F <my-account2> -t=unsigned.txn
    ```

    Merge signings 
    ```bash
    goal clerk multisig merge --out signed.txn unsigned.txn
    ```

    Broadcast

    ```bash
    goal clerk rawsend --filename signed.txn
    ```