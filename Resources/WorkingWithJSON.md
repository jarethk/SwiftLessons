# Working With JSON
JSON makes a great data/resource format, with a wide variety of potential uses, easily created/edited, and potential for use with web services.

## Codable
Classes and Structs can be decoded easily 
 - Extend `Codable`
 - Define `CodingKeys` mapping fields to json property names
 - Implement `required init(from decoder: Decoder)` method to decode fields

## DecodingContainer
The `init` method executes in 3 phases:
 - Instantiate the container `let container = try decoder.container(keyedBy: CodingKeys.self)`
 - Decode the fields `self.name = try container.decode(String.self, forKey: .name)`
 - If applicable, for types that extend other types, decode the parent type too `try super.init(from: decoder)`

## Resource Bundles
JSON resource files stored locally within the project are best accessed as a Bundle. Fortunately `Bundle` can be extended for decoding the JSON file as shown below, courtesy of Hacking With Swift (d).

```SWIFT
extension Bundle {
    /**
     Extension to Bundle to enable easy decoding of JSON files.  Includes proper messaging for all error varieties.
     */
    func decodeJson<T: Decodable>(_ type: T.Type, from file: String, dateDecodingStrategy: JSONDecoder.DateDecodingStrategy = .deferredToDate, keyDecodingStrategy: JSONDecoder.KeyDecodingStrategy = .useDefaultKeys) -> T {
        guard let url = self.url(forResource: file, withExtension: nil) else {
            fatalError("Failed to locate \(file) in bundle.")
        }

        guard let data = try? Data(contentsOf: url) else {
            fatalError("Failed to load \(file) from bundle.")
        }

        let decoder = JSONDecoder()
        decoder.dateDecodingStrategy = dateDecodingStrategy
        decoder.keyDecodingStrategy = keyDecodingStrategy

        do {
            return try decoder.decode(T.self, from: data)
        } catch DecodingError.keyNotFound(let key, let context) {
            fatalError("Failed to decode \(file) from bundle due to missing key '\(key.stringValue)' not found – \(context.debugDescription)")
        } catch DecodingError.typeMismatch(_, let context) {
            fatalError("Failed to decode \(file) from bundle due to type mismatch – \(context.debugDescription)")
        } catch DecodingError.valueNotFound(let type, let context) {
            fatalError("Failed to decode \(file) from bundle due to missing \(type) value – \(context.debugDescription)")
        } catch DecodingError.dataCorrupted(_) {
            fatalError("Failed to decode \(file) from bundle because it appears to be invalid JSON")
        } catch {
            fatalError("Failed to decode \(file) from bundle: \(error.localizedDescription)")
        }
    }
}

```

Within projects preference is to locate the JSON files within a `Resources` folder.

## References
 - [a](https://www.avanderlee.com/swift/json-parsing-decoding/)
 - [b](https://stablekernel.com/article/understanding-extending-swift-4-codable/)
 - [c](https://www.hackingwithswift.com/articles/119/codable-cheat-sheet)
 - [d](https://www.hackingwithswift.com/example-code/system/how-to-decode-json-from-your-app-bundle-the-easy-way)
