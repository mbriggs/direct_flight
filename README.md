# Direct Flight

When using s3, most people upload to their app, the app processes the file in some way, then the app uploads the final result to s3

![S3 Flow](/readme-graphs/normal-flow.png?raw=true "Normal S3 Flow")

This is the simplest approach, and the one you should use, unless for some reason you cant based on the resource constraints of your app server (for example, uploading large files to heroku). This is where direct flight kicks in

![New S3 Flow](/readme-graphs/direct-flight-flow.png?raw=true "Direct Flight S3 Flow")

In this case, the browser gets credentials from the app server, uses them to upload to s3, and then tells the app server about the result.


## Installation

The package can be installed as:

  1. Add `direct_flight` to your list of dependencies in `mix.exs`:

    ```elixir
    def deps do
      [{:direct_flight, "~> 0.1.0"}]
    end
    ```

  2. Ensure `direct_flight` is started before your application:

    ```elixir
    def application do
      [applications: [:direct_flight]]
    end
    ```

## How to implement a Direct Flight to S3

![Direct Flight sequence of events](/readme-graphs/direct-flight-sequence.png?raw=true "Direct Flight Event Sequence")

For the processing side, a library like arc would make it quite easy to implement.

## Missing features I will happily take pull requests for

This solves my use case, but there are a few things that probably should be there

 - Remove ExAws dependency: I figure if you are using aws you likely have this library already installed and configured. All I am using it for is the signing algorithm, and piggybacking off its s3 configuration. 
 - Add forms based helpers: I am using XHR based file uploads, but if you dont have a javascript heavy app, you probably want to actually use html forms for the upload itself. It would not be difficult to generate the html based on the returned data. However, I do not want to take on a phoenix dependency, so please don't use their helpers to do it.
