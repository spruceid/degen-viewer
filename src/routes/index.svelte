<script lang="ts">
	import { polyfill } from './../CredentialWallet';
  import type { TableColumn } from "./../components/table/table_columns/TableColumn";
  import Table from "../components/table/Table.svelte";
  import ImageColumn from "../components/table/table_columns/ImageColumn.svelte";
  import Button from '../components/table/Button.svelte';

  let errorMessage: string;
  let statusMessage: string = "";
  let vps: Array<any> = [];
	$: userSuppliedID = "";

  let tableColumns: Array<TableColumn> = [
    { header: { title: "ID" }, path: "id" },
    { header: { title: "Type" }, path: "type" },
    { header: { title: "Proof" }, path: "proof.type" },
    { header: { title: "Expiry" }, path: "expires", transform: (content, _) => { console.log(content); return content.expires || "none" }},
    { header: { title: "Issuer" }, path: "issuer" }
  ];

  async function getCredentials() {
    errorMessage = "";
    statusMessage = "Waiting for Credential Wallet...";
    try {
      const query = {
        query: [{
          type: "QueryByExample",
          credentialQuery: [
            {
              reason: "Proof of ownership for ETHR",
              example: {
                credentialSubject: {
                  id: "did:tz:"
                }
              }
            },
            {
              reason: "Proof of liquidity for ETHR",
              example: {
                credentialSubject: {
                  id: "did:tz:"
                }
              }
            },
            {
              reason: "Proof of assets for ETHR",
              example: {
                credentialSubject: {
                  id: "did:tz:"
                }
              }
            }
          ]
        }],
        challenge: "uuid",
        domain: "domain.tld"
      };

      const vp: any = await polyfill.get(query);
      if(Array.isArray(vp.verifiableCredential))
        vps = vp.verifiableCredential;
      else
        vps = [...vps, vp.verifiableCredential];
      console.log(vp)
      statusMessage = "";
    } catch (err) {
      statusMessage = "";
      console.error(err);
      errorMessage = err.message;
    }
  }
</script>

<svelte:head>
  <title>Credentials</title>
</svelte:head>

<div class="absolute top-4 left-4 z-20">
	<Button label="Connect Credential Wallet" onClick={getCredentials} />
</div>

<div class="flex flex-col w-2/3">
  {errorMessage || statusMessage}
  <Table
    fluid
    elements={vps}
    columns={tableColumns}
  />
</div>
