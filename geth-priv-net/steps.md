1. Generate a new Ethereum Account to use it as the Signer.

    `geth account new --datadir node1`


2. Copy the address of the generated account (as shown in the terminal).

3. Modify the genesis.json to include the address as a Signer Account.
    1. In the `extradata` field, add `0x`
    2. Then add 32 zero bytes, i.e., 0000....0000 after `0x`
    3. Then add the address of the account generated without the `0x`
    4. Then add 65 more zero bytes after the address

    ```    "difficulty": "1",
        "gasLimit": "8000000",
        "extradata": "0x0000000000000000000000000000000000000000000000000000000000000000e03ad86b8fd3c47f16863d70f75500128dd9ca5b0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
        "alloc": {
            "e03ad86b8fd3c47f16863d70f75500128dd9ca5b": {
                "balance": "300000"
            },
            "f41c74c9ae680c1aa78f42e5647a62f353b7bdde": {
                "balance": "400000"
            }
        }
    }

4. Initialize the Geth Database by using

    `geth init --datadir node1 genesis.json`


5. Start the Geth Node-1 using the command:
    
    `geth --datadir node1 --networkid 824032 --unlock 0x232C6611680b5993432Fbf35fb94005cAa3AAbd3 --mine --miner.etherbase 0x232C6611680b5993432Fbf35fb94005cAa3AAbd3`

6. Start the Javascript Console for Node-1:

    `geth attach node1/geth.ipc`

7. Check the balance of the signer account (in the js console):

    `eth.getBalance("0x232C6611680b5993432Fbf35fb94005cAa3AAbd3")`

8. Get the Node Record of Node-1 by executing the following in the JS Console:

    `admin.nodeInfo.enr`

9. Init the Second Node:

    `geth init --datadir node2 genesis.json`

10. Start the second node, i.e., Node-2:

    `geth --datadir node2 --networkid 824032 --port 30306 --authrpc.port 8553  --bootnodes "enr:-KO4QGb73xzGsdK3lt9mw2AZn2152s5bHdTxeo4R8wGpQI3Lb7cNcTRDIbO3a9ek3I47UdzDJbOSDeTREm7NCvPdwqmGAYpqc83fg2V0aMfGhIw59tiAgmlkgnY0gmlwhH8AAAGJc2VjcDI1NmsxoQLNv435unhWOJrKT4w_Z6WAPDOYuapz0YnfEL24rH-_DYRzbmFwwIN0Y3CCdl-DdWRwgnZf"`

11. Start the JS Console for Node-2:

    `geth attach node2/geth.ipc`

12. Check in Node-2 console for other peers:

    `admin.peers`

13. In Node-2 Try to create a new account (using Terminal):

    `geth account new --datadir node2`

14. Start the JS Console of Node-2 and Show the newly added account:

    `eth.accounts`

15. From Node-1's account, send some ether to Node-2's account:

    `eth.sendTransaction({from: '0x232C6611680b5993432Fbf35fb94005cAa3AAbd3', to: '0x619c170cd5979d13c0aa3c6738378094e39b4d87', value: 5000})`

16. Check the balance of the newly created account on Node-2:

    `eth.getBalance("0x619c170cd5979d13c0aa3c6738378094e39b4d87")`