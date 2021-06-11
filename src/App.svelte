<script>
  import {
    Content,
    DataTable,
    Button,
    Grid,
    Row,
    Column,
    Tabs,
    TabContent,
    Tab,
    CodeSnippet,
    Select,SelectItem,
    TextInput,
    Form,FormGroup,
  } from "carbon-components-svelte";
  import TrashCan16 from "carbon-icons-svelte/lib/TrashCan16";
  import Header from "./components/Header.svelte";
  let url;
  let site = "";
  let metafield = "";
  let resource = "Product";
  let loading = false;
  let disabled = true;
  let invalid = false;

   const headers = [
    { key: "name", value: "Metafield" },
    { key: "overflow", empty: true },
  ];

  let metafields = [{name: "Colour", id: "colour"}, {name: "Designer name", id: "designer_name"}];

  function generateMetafields() {
    const d = metafields.map(e => `metafields.global.${e.id}`);
    return d.join('%2C');
  }

  function removeMetafield(row) {
      metafields = metafields.filter(m => m.id !== row.id);
  }

  function addMetafield() {
      const RegExpression = /^[a-zA-Z\s]*$/;

      if (!metafield || !RegExpression.test(metafield)) {
          invalid = true;
          return;
      }

      const name = metafield.trim();
      const id = name.toLowerCase().replace(/\s/g, "_");

       if (metafields.find(e => e.id === id)) {
          invalid = true;
          return;
      }

      metafields = [...metafields, {name, id}]

      metafield = "";

  }
$: {
    if (site && metafields.length > 0) {
        url = `${site.replace(/\/$/, "")}/admin/bulk?resource_name=${resource}&edit=${generateMetafields()}`;
        disabled = false;
    } else {
        url = "First you need to add site and metafields..."
        disabled = true;
    }
};
 $: document.documentElement.setAttribute("theme", "g10");
</script>

  <Header />
  <Content style="background: none; padding: 1rem">
    <Grid>
      <Row style="padding-top: 2rem;">
        <Column lg="{16}">
          <h1 style="margin-bottom: 1.5rem">Generate Shopify Metafields Editor Link</h1>
          <p style="margin-bottom: 1.5rem">Generate the link needed to access custom metafields for products in your Shopify Store without the need for a plugin or app.</p>
          <h5 style="margin-bottom: 1rem">Copy the admin URL</h5>
                      <CodeSnippet size="xl"  code={url} skeleton={loading} {disabled} hideCopyButton={disabled} />
        </Column>
      </Row>

      <Row style="margin-top: 1rem">
        <Column noGutter>
          <Tabs aria-label="Tab navigation">
            <Tab label="Generator" />
            <Tab label="Why" />
            <Tab label="How" />
            <div slot="content" class="tabbed-content">
              <Grid as fullWidth let:props>
                <TabContent {...props}>
                  <Row>
                    <Column md={5} >

                        <TextInput bind:value={site}
                       size="xl" required invalidText="This field is requried" labelText="Your shopify URL" placeholder="Shopify URL"
  helperText="This can be your site's domain name or your actual myshopify URL. E.g. 'https://my-coffee-store.myshopify.com' or 'https://coffestore.com'"/>

   <Select  style="margin-top: 1rem"
                        labelText="Resource" size="xl"
                        bind:selected="{resource}"
                      >
                        <SelectItem value="Product" text="Product" />
                        <SelectItem value="Collection" text="Collection" />
                      </Select>

                      <h5 style="margin-top: 1rem">Metafields</h5>
                      <p>Add here the custom meta fields you wish to be able to manage.</p>
                      {#if metafields.length > 0}
                          <DataTable  {headers} rows={metafields}>
                            <span slot="cell" let:cell let:row>
                                {#if cell.key === 'overflow'}
                                <Button kind="danger" iconDescription="Remove" on:click={() => removeMetafield(row)} icon={TrashCan16} />
                                {:else}{cell.value}{/if}
                            </span>
                            </DataTable>
                      {/if}
                      <Form style="margin-top: 1rem" on:submit={addMetafield}>

                      <h5 style="margin-top: 1rem">Add new metafield</h5>
                             <TextInput bind:value={metafield}
                       size="xl" on:change={() => {invalid = false}} {invalid} invalidText="This field is requried and should be unique. Keep it simple. Letters and spaces only." labelText="Field name" placeholder="e.g. Colour, Designer name"
  helperText="Keep it simple. Letters and spaces only."/>
  <Button  style="margin-top: 1rem" type="submit">Add Metafield</Button>

                      </Form>


                    </Column>
                  </Row>
                </TabContent>
                <TabContent {...props}>
                  <Row>
                    <Column md={5} >
                      <p>
                        Was watching "Code with Chris" and his video on <a href="https://www.youtube.com/watch?v=UbwhADWZzvQ" target="_blank">Custom Fields in Shopify</a>, and thought it would be good to have such tool available to simplify access to metafields in the Shopify Admin.
                      </p>
                    </Column>
                  </Row>
                </TabContent>
                <TabContent {...props}>
                  <Row>
                    <Column md={5} >
                        <p style="margin-bottom: 1rem">
                        After you have added your custom metafields, you can simply include them in your theme code in liquid as following:

                      </p>
                      <CodeSnippet  style="margin-bottom: 1rem" code={"{{ product.metafields.global.field_name }}"}   />
                      <p style="margin-bottom: 1rem">
                        This solution has a few limitations, as you are limited to only using global metafields, and you can't nest metafield objects.
                      </p>
                      <p style="margin-bottom: 1rem">
                        This tool is public repo available at <a href="https://github.com/imCorfitz/shopify-metafields-editor-link" target="_blank">https://github.com/imCorfitz/shopify-metafields-editor-link</a>
                      </p>
                    </Column>
                  </Row>
                </TabContent>
              </Grid>
            </div>
          </Tabs>
        </Column>
      </Row>
    </Grid>
  </Content>
