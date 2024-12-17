[![Go Report Card](https://goreportcard.com/badge/github.com/hippopos/geoserver)](https://goreportcard.com/report/github.com/hippopos/geoserver)
[![GitHub license](https://img.shields.io/github/license/hishamkaram/geoserver.svg)](https://github.com/hippopos/geoserver/blob/master/LICENSE)
[![GitHub issues](https://img.shields.io/github/issues/hishamkaram/geoserver.svg)](https://github.com/hippopos/geoserver/issues)
[![Coverage Status](https://coveralls.io/repos/github/hishamkaram/geoserver/badge.svg?branch=master&service=github)](https://coveralls.io/github/hishamkaram/geoserver?branch=master&service=github)
[![Build Status](https://travis-ci.org/hishamkaram/geoserver.svg?branch=master)](https://travis-ci.org/hishamkaram/geoserver)
[![Documentation](https://godoc.org/github.com/hippopos/geoserver?status.svg)](https://godoc.org/github.com/hippopos/geoserver?)
[![GitHub forks](https://img.shields.io/github/forks/hishamkaram/geoserver.svg)](https://github.com/hippopos/geoserver/network)
[![GitHub stars](https://img.shields.io/github/stars/hishamkaram/geoserver.svg)](https://github.com/hippopos/geoserver/stargazers)
[![Twitter](https://img.shields.io/twitter/url/https/github.com/hippopos/geoserver/edit/master/README.md.svg?style=social)](https://twitter.com/intent/tweet?text=Wow:&url=https%3A%2F%2Fgithub.com%2Fhishamkaram%2Fgeoserver%2Fedit%2Fmaster%2FREADME.md)





<p align="center">
  <img src="https://i.imgur.com/bVuV5v6.png" width="200"/>
</p>
<p align="center">
  <img src="https://i.imgur.com/31CL1xg.png" width="200"/>
</p>

# Geoserver
geoserver Is a Go Package For Manipulating a GeoServer Instance via the GeoServer REST API. 

---
## How to install:
- `go get -v gopkg.in/hishamkaram/geoserver.v1`
  - now you can import the package from `gopkg.in/hishamkaram/geoserver.v1`, example:
    ```
    import (
      ...
      "gopkg.in/hishamkaram/geoserver.v1"
      ...
    )
    ```
---

## usage:
  - Create new Catalog (which contains all available operations):
      - `gsCatalog := geoserver.GetCatalog("http://localhost:8080/geoserver13/", "admin", "geoserver")`
  - Use catalog Methods to Perform a Geoserver REST Operation:
      - Create New workspace:
        ```
        created, err := gsCatalog.CreateWorkspace("golang")
        if err != nil {
          fmt.Printf("\nError:%s\n", err)
        }
        fmt.Println(strconv.FormatBool(created))
        ```
        output if created:
        ```
        INFO[31-03-2018 16:26:35] url:http://localhost:8080/geoserver13/rest/workspaces	response Status=201  
        true
        ```
        output if error:
        ```
        INFO[31-03-2018 16:26:37] url:http://localhost:8080/geoserver13/rest/workspaces	response Status=401  
        WARN[31-03-2018 16:26:37] <!doctype html><html lang="en"><head><title>HTTP Status 401 – Unauthorized</title><style type="text/css">h1 {font-family:Tahoma,Arial,sans-serif;color:white;background-color:#525D76;font-size:22px;} h2 {font-family:Tahoma,Arial,sans-serif;color:white;background-color:#525D76;font-size:16px;} h3 {font-family:Tahoma,Arial,sans-serif;color:white;background-color:#525D76;font-size:14px;} body {font-family:Tahoma,Arial,sans-serif;color:black;background-color:white;} b {font-family:Tahoma,Arial,sans-serif;color:white;background-color:#525D76;} p {font-family:Tahoma,Arial,sans-serif;background:white;color:black;font-size:12px;} a {color:black;} a.name {color:black;} .line {height:1px;background-color:#525D76;border:none;}</style></head><body><h1>HTTP Status 401 – Unauthorized</h1><hr class="line" /><p><b>Type</b> Status Report</p><p><b>Message</b> Workspace &#39;golang&#39; already exists</p><p><b>Description</b> The request has not been applied because it lacks valid authentication credentials for the target resource.</p><hr class="line" /><h3>Apache Tomcat/9.0.6</h3></body></html> 

        Error:Unauthorized
        false
        ```
  - Get Layers through GetLayers take workspace as paramter if empty workspace will be ignored and geoserver will return all public layers
      ```
      layers, err := gsCatalog.GetLayers("")
      if err != nil {
        fmt.Printf("\nError:%s\n", err)
      }
      for _, lyr := range layers {
        fmt.Printf("\nName:%s  href:%s\n", lyr.Name, lyr.Href)
      }
      ```
      output:
      ```
      INFO[31-03-2018 19:04:44] url:http://localhost:8080/geoserver13/rest/layers	response Status=200  

      Name:tiger:giant_polygon  href:http://localhost:8080/geoserver13/rest/layers/tiger%3Agiant_polygon.json

      Name:tiger:poi  href:http://localhost:8080/geoserver13/rest/layers/tiger%3Apoi.json

      Name:tiger:poly_landmarks  href:http://localhost:8080/geoserver13/rest/layers/tiger%3Apoly_landmarks.json

      Name:tiger:tiger_roads  href:http://localhost:8080/geoserver13/rest/layers/tiger%3Atiger_roads.json

      Name:nurc:Arc_Sample  href:http://localhost:8080/geoserver13/rest/layers/nurc%3AArc_Sample.json

      Name:nurc:Img_Sample  href:http://localhost:8080/geoserver13/rest/layers/nurc%3AImg_Sample.json

      Name:nurc:Pk50095  href:http://localhost:8080/geoserver13/rest/layers/nurc%3APk50095.json

      Name:nurc:mosaic  href:http://localhost:8080/geoserver13/rest/layers/nurc%3Amosaic.json
      ......
      ```
  - Get Specific Layer from Geoserver:
      ```
      layer, err := gsCatalog.GetLayer("nurc", "Arc_Sample")
      if err != nil {
        fmt.Printf("\nError:%s\n", err)
      } else {
        fmt.Printf("%+v\n", layer)
      }
       ```
       output:
       ```
      INFO[31-03-2018 20:12:07] url:http://localhost:8080/geoserver13/rest/workspaces/nurc/layers/Arc_Sample	response Status=200  
      {Name:Arc_Sample Path:/ Type:RASTER DefaultStyle:{Class: Name:rain Href:http://localhost:8080/geoserver13/rest/styles/rain.json} Styles:{Class:linked-hash-set Style:[{Class: Name:raster Href:http://localhost:8080/geoserver13/rest/styles/raster.json}]} Resource:{Class:coverage Name:nurc:Arc_Sample Href:http://localhost:8080/geoserver13/rest/workspaces/nurc/coveragestores/arcGridSample/coverages/Arc_Sample.json} Queryable:false Opaque:false Attribution:{Title: Href: LogoURL: LogoType: LogoWidth:0 LogoHeight:0}}
       ```
  - You can find more examples by check testing files
  - You can find all supported operations on [Godocs](https://godoc.org/github.com/hippopos/geoserver)
  ---

### TESTING
|   | Go Version | Geoserver Version | Tested             |
|---|------------|-------------------|--------------------|
| 1 | 1.13.x      | 2.13.x            | :heavy_check_mark: |
| 2 | 1.13.x      | 2.14.x            | :heavy_check_mark: |
| 3 | 1.14.x     | 2.13.x            | :heavy_check_mark: |
| 4 | 1.14.x     | 2.14.x            | :heavy_check_mark: |
| 5 | 1.15.x     | 2.13.x            | :heavy_check_mark: |
| 6 | 1.15.x     | 2.14.x            | :heavy_check_mark: |

___
### [Documentation](https://godoc.org/github.com/hippopos/geoserver)