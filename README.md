# send_mail

![img alt](https://github.com/ComputerScienceHayk/flutter_send_mail/blob/master/screen.png)

flutter send mail

## Getting Started

1) add 'flutter_email_sender: ^3.0.1' to pubspec.yaml 
  ...
dependencies:
  flutter:
    sdk: flutter
  flutter_email_sender: ^3.0.1
  ...
  
 2) create SendMail.dart
 
  import 'dart:async';
  import 'package:flutter/material.dart';
  import 'package:flutter_email_sender/flutter_email_sender.dart';

  class SendMail extends StatefulWidget {
    @override
    _SendMailState createState() => _SendMailState();
  }

  class _SendMailState extends State<SendMail> {

    final _recipientController = TextEditingController(
      text: 'nowunocry@gmail.com',
    );

    final _subjectController = TextEditingController(text: '');

    final _bodyController = TextEditingController(
      text: '',
    );

    final GlobalKey<ScaffoldState> _scaffoldKey = GlobalKey<ScaffoldState>();

    List<String> attachments = [];

    Future<void> send() async {
      final Email email = Email(
        body: _bodyController.text,
        subject: _subjectController.text,
        recipients: [_recipientController.text],
        attachmentPaths: attachments,
      );

      String platformResponse;

      try {
        await FlutterEmailSender.send(email);
        platformResponse = 'success';
      } catch (error) {
        platformResponse = error.toString();
      }

      if (!mounted) return;

      _scaffoldKey.currentState.showSnackBar(SnackBar(
        content: Text(platformResponse),
      ));
    }

    Widget _buildSubject() {
      return Container(
        padding: EdgeInsets.only(left: 20, right: 20, top: 90, bottom: 10),
        child: TextFormField(
          controller: _subjectController,
          decoration: new InputDecoration(
              focusedErrorBorder:new OutlineInputBorder(
                  borderSide: new BorderSide(color: Colors.red[400])
              ),
              focusedBorder:new OutlineInputBorder(
                  borderSide: new BorderSide(color: Colors.teal)
              ),
              border: new OutlineInputBorder(
                  borderSide: new BorderSide(color: Colors.green[400])
              ),
              labelText: 'Subject',
              labelStyle: TextStyle(
                  color: Colors.teal
              ),
              fillColor: Colors.grey[100],
              filled: true,
              prefixIcon: const Icon(
                Icons.subject,
                color: Colors.teal,
              ),
              suffixStyle: const TextStyle(color: Colors.green)),
          maxLength: 20,
          validator: (String value) {
            if (value.isEmpty) {
              return 'Subject is Required';
            } else if (value.length < 3) {
              return 'Subject is short';
            }

            return null;
          },
        ),
      );
    }


    Widget _buildBody() {
      return Container(
        padding: EdgeInsets.only(left: 20, right: 20, top: 10, bottom: 10),
        child: TextFormField(
          textAlignVertical: TextAlignVertical(y: 1.0),
          controller: _bodyController,
          minLines: 10,
          maxLines: null,
          keyboardType: TextInputType.multiline,
          decoration: new InputDecoration(
              alignLabelWithHint: true,
              hintStyle: TextStyle(
                  color: Colors.teal
              ),
              prefixIcon: Container(
                padding: EdgeInsets.only(top: 2.0),
                child: Icon(
                  Icons.text_fields,
                  color: Colors.teal,
                ),
              ),
              contentPadding: EdgeInsets.only(top: 30.0,left: 5.0, right: 5.0, bottom: 20.0),
              focusedErrorBorder:new OutlineInputBorder(
                  borderSide: new BorderSide(color: Colors.red[400])
              ),
              focusedBorder:new OutlineInputBorder(
                  borderSide: new BorderSide(color: Colors.teal)
              ),
              border: new OutlineInputBorder(
                  borderSide: new BorderSide(color: Colors.green[400])
              ),
              hintText: "Your offer",
              labelText: 'Body',
              labelStyle: TextStyle(
                  color: Colors.teal
              ),
              fillColor: Colors.grey[100],
              filled: true,
              suffixStyle: const TextStyle(color: Colors.green)
          ),
          maxLength: null,
          validator: (String value) {
            if (value.isEmpty) {
              return 'Body is Required';
            }

            return null;
          },
        ),
      );
    }

    @override
    Widget build(BuildContext context) {
      return Scaffold(
        body: Center(
          child: SingleChildScrollView(
            child: Padding(
              padding: EdgeInsets.all(8.0),
              child: Column(
                mainAxisSize: MainAxisSize.max,
                crossAxisAlignment: CrossAxisAlignment.center,
                children: <Widget>[
                  Align(
                    child: Text("Flutter Send Mail", style: TextStyle(color: Colors.teal, fontSize: 25.0),),
                  ),
                  _buildSubject(), // Subject
                  _buildBody(), // Body
                  ...attachments.map(
                        (item) => Text(
                      item,
                      overflow: TextOverflow.fade,
                    ),
                  ),
                  Container(
                    padding: EdgeInsets.only(left: 20.0, right: 20.0, top: 10.0, bottom: 10.0),
                    margin: EdgeInsets.only(left: 130.0, right: 130.0, top: 20.0),
                    decoration: BoxDecoration(
                        color: Colors.grey[100],
                        border: Border.all(color: Colors.teal),
                        borderRadius: BorderRadius.all(Radius.circular(5.0))
                    ),
                    child: InkWell(
                      onTap: send,
                      child: Row(
                        mainAxisAlignment: MainAxisAlignment.spaceBetween,
                        children: [
                          Text("Send", style: TextStyle(color: Colors.teal),),
                          Icon(Icons.send, color: Colors.teal,),
                        ],
                      ),
                    ),
                  ),
                ],
              ),
            ),
          ),
        ),
      );
    }

  }
