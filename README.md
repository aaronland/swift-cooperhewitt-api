# CooperHewittAPI

Swift package for working with the Cooper Hewitt API.

## Example

```
import CooperHewittAPI

func WWMS(access_token: String){
    
    let api = CooperHewittAPI(access_token: access_token)
    let method = "cooperhewitt.labs.whatWouldMicahSay"
    let params = [String:String]()
        
    api.ExecuteMethod(method: method, params: params, completion: doWWMS)
}

func doWWMS(rsp: Result<CooperHewittAPIResponse, Error>) {
    
    switch rsp {
    case .failure(let error):
        
        switch error {
        case is CooperHewittAPIError:
            let api_error = error as! CooperHewittAPIError
            print(api_error.Message)
        default:
            print(error.localizedDescription)
        }

        return
        
    case .success(let api_rsp):
        
        struct WWMSResponse: Codable {
            var micah: WWMS
        }

        struct WWMS: Codable {
            var says: String
        }
        
        let decoder = JSONDecoder()
        var wwms: WWMSResponse
        
        do {
            wwms = try decoder.decode(WWMSResponse.self, from: api_rsp.Data)
        } catch(let error) {
            print(error)
            return
        }
        
        print(wwms.micah.says)
    }
}
```

## See also

* https://collection.cooperhewitt.org/api
