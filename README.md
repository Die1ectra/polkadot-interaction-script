# polkadot-interaction-script
 This script leverages the Polkadot API to perform basic actions, such as querying blockchain information and submitting transactions.
from substrateinterface import SubstrateInterface, Keypair
from datetime import datetime

class PolkadotInteractionScript:
    def __init__(self, node_url, keypair):
        self.substrate = SubstrateInterface(url=node_url)
        self.keypair = keypair

    def query_block_info(self, block_number):
        # Query information about a specific block
        block_info = self.substrate.get_block(block_number)
        print(f"Block {block_number} Information:")
        print(f"Timestamp: {datetime.utcfromtimestamp(block_info['timestamp'])}")
        print(f"Hash: {block_info['hash']}")
        print(f"Author: {block_info['author']}")
        print("-" * 30)

    def submit_transaction(self, recipient, amount):
        # Submit a simple balance transfer transaction
        call = self.substrate.compose_call(
            call_module='Balances',
            call_function='transfer',
            call_params={'dest': recipient, 'value': amount}
        )

        extrinsic = self.substrate.create_signed_extrinsic(call=call, keypair=self.keypair)
        self.substrate.submit_extrinsic(extrinsic, wait_for_inclusion=True)
        print(f"Transaction submitted: {amount} DOT sent to {recipient}")

# Example Usage
node_url = "wss://rpc.polkadot.io"
alice_keypair = Keypair.create_from_mnemonic("your_alice_mnemonic_here")

polkadot_script = PolkadotInteractionScript(node_url, alice_keypair)

# Query block information
polkadot_script.query_block_info(12345)

# Submit a transaction
polkadot_script.submit_transaction("recipient_address_here", 10)

This script provides a simple interface for interacting with the Polkadot blockchain. Users can query block information and submit transactions. Developers can build upon this script to create more complex applications and explore the diverse features of the Polkadot network.
