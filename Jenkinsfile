#!/usr/bin/groovy
@Library('github.com/fabric8io/osio-pipeline@master')_

osio {
  config runtime: 'node'

  ci {
    def resources = processTemplate()
    build resources: resources
  }



  cd {
    def resources = processTemplate(params: [
            release_version: "1.0.${env.BUILD_NUMBER}"
    ])

    echo "-------------- build default ----------------------------"
    build resources: resources
    echo "-------------- build separate ----------------------------"
    build resources: [
            [ BuildConfig: resources.BuildConfig],
            [ImageStream: resources.ImageStream],
    ]

    // echo "-------------- build commands ---------------------------"
    // // test passing in commands
    // build app: app, commands: """
    //   npm version
    //   oc version
    // """

    echo "-------------- deploy -----------------------------------"
    //deploy resources: resources, env: 'stage'

    deploy resources: resources, env: 'run', approval: 'manual'
    echo "---------------------------------------------------------"
  }
}
