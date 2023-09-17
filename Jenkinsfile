pipeline {

    agent any

 

    stages {

        stage('Build') {

            steps {

                script {

                    def usernameToApprove = 'specific_user' // Replace with the actual username


                    // Prompt for user input

                    def userInput = input(

                        id: 'userInput', 

                        message: 'Do you want to proceed with the build?',

                        ok: 'Proceed',

                        submitter: usernameToApprove, // Display the button only to the specified user

                        parameters: [

                            choice(

                                name: 'PROCEED',

                                choices: 'Yes\nNo',

                                description: 'Choose whether to proceed'

                            )

                        ]

                    )

                                   def css = """

        <style>

            .proceed-button{

    display: none !important;

}

        </style>

    """

 

                    // Check the user's input

                    if (userInput == 'Yes') {

                        echo 'Building...'

                        // Add build steps here

                    } else {

                        echo 'Build not proceeding.'

                    }

                }

            }

        }

    }


    post {

        always {

            echo 'Build finished'

        }

    }

}
