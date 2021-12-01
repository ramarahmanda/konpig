# Konpig

A configurable modern config. This project aims to accelerated operational request in your company such as changing text copy, a/b testing, rules engine, etc by defining every changing variable using multiple data source such as google sheet, APIs. 

By creating this, I hope I can free you guys (developer) from annoying operational tasks and make them self service for what they need!

## Component
This project architecture contains of
- Entry
    ```
    how you will access your configuration
    ```
- Source (optional) 
    ```
    multiple data sources can be used to fuel your config, make it dynamic!
    ```
- Transformer (optional)
    ```
    you can put logic to pre-compute your data source before sending it to entry point
    ```

## How to use
1. Create a yaml config file with unique name, this will be used as entry point to your config
 - example: `shitty-banner.yaml`
```yaml
sources:
    - type: gsheet
      credential: ..
      url: ..
    - type: api
      method: ..
      url: ..
      headers: ..
      body: ..
transformer:
    type: fn
    fn: `(sources) => sources`
entry:
    api:
        type: json
        data:
            isShowBanner: {{sources[0].status}}
            banners:
                - bannerName: {{sources[0].name}}
                  bannerId: {{sources[0].id}}
                - bannerName: {{sources[1].name}}
                  bannerId: {{sources[1].id}}
```
2. Your config will be accessible using REST API
```bash
curl --location --request GET '{{konpig_host}}/shitty-banner'
```
3. Response
```
{
    isShowBanner: false,
    banners: [
        ..,
        {
          bannerName: ..,
          bannerId: ..    
        }]
}
```