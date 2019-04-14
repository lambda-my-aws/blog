.. title: Ozone - a library for Lambda-my-AWS
.. slug: ozone-a-library-for-lambda-my-aws
.. date: 2019-04-14 19:40:34 UTC
.. tags: aws,cloudformation,lambda,troposphere,ozone
.. category: aws,cloud
.. link: https://github.com/lambda-my-aws/ozone
.. description: Why Ozone
.. type: text

Ozone
=====

Ozone was born after I started to work with Troposphere. The original idea was to call it Stratosphere, however the pypi is empty, it was taken. Trying to get inspired by my #2 favorite viodeo game, `Kerbal Space Program <https://www.kerbalspaceprogram.com/>`_, I ended up using Ozone, which is the layer keeping us all from frying of magnetic radiations, on Earth.

The original plan was to have a python module per specific purpose, and it might still end up into a split later down the line. But given these libs were using each other very tightly, I eneded up bundling things together within this one package.

What is the purpose of Ozone?
-----------------------------

Ozone is a swiss knife to making "Lambda my AWS" as easy as possible to realize: each module serves a purpose that others can re-use. Let's go over the modules

filters
^^^^^^^

The filters are a collection of regexes and functions which are going to check that the info passed on, whether directly within a Lambda function or to a Troposphere object is correct. For now, the `arns.py` filter allows both to enforce for the ARN to be valid when full ARN given, or, return an ARN for an object using `Fn::Sub` around the account ID and region where the stack will be deployed.

handlers
^^^^^^^^

This probably will end-up as a library of its own. The idea of the handlers modules is to provide a lexer, parser and responder to a Lambda function based on what's asked of it and what service is making the call to it (ie. distinguish SNS is the sender vs. CloudFormation).

It was primarily designed to handle Cloudformation Stacks creations for Lambda-my-AWS template functions.

resolvers
^^^^^^^^^

The resolvers are a series of functions that can be used individually or in group to resolve items on AWS. For example, `find_org_in_tree` in `organizations` will find the organization based on its path (ie, 'app1/prod').

Functions in the resolvers section are present mostly because they were needed in existing lambda functions you will find in the lambda-my-aws organization.


outputs
^^^^^^^

This is there the Troposphere part of things comes in: with Troposphere, we can use Python to generate Cloudformation templates which gives the perfect gateway to re-usability of functions instead of having to manage hundreds of specific Cloudformation templates, in many S3 buckets.

The outputs are simply a wrapper around the `Output` object in Troposphere allowing to simply passing in the Troposphere object we want outputs for, and format Cloudformation `Exports` values to provide as much consistency as possible

resources
^^^^^^^^^

Same as above, this is a more Troposhere specific module: a lot of the resources that people are going to create, like an S3 bucket, you end up writing in Python pretty the exact same thing as you would have in native JSON or YAML. With Ozone, you can simply do

.. code-block:: python

		from ozone.resources.s3.bucket import S3Bucket
		bucket = S3Bucket('my-bucket', UseEncryption=True, UseVersioning=True)



templates
^^^^^^^^^

This module is there to achieve two objectives: give examples on how to use the lib and provide with some default templates which are used all over the place for automation of various parts of the release and unit-testing of the lambda functions in lambda-my-aws.

