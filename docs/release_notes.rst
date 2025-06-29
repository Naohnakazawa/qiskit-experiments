=============
Release Notes
=============

.. release-notes::
   :earliest-version: 0.11.0

.. _Release Notes_0.10.0:

0.10.0
======

.. _Release Notes_0.10.0_Prelude:

Prelude
-------

.. releasenotes/notes/prelude0.10-39fb8503056ee261.yaml @ b'c7d41beb5693db3b719fb00f7f9611433ac61c15'

The Qiskit Experiments 0.10.0 release adds new features to :class:`.LayerFidelity` that allow it to be used as a parallel direct randomized benchmarking experiment as well. It also includes some small changes to address the way :func:`~qiskit.compiler.transpile` handles the ``basis_gates`` argument in Qiskit 2.0.


.. _Release Notes_0.10.0_New Features:

New Features
------------

.. releasenotes/notes/layerfid_opt_add-a42d6e727e5d44b9.yaml @ b'4c7f2b448d603c2d36e93dfa212d14f05dd8763f'

- :class:`.LayerFidelity` has two new options, ``layer_barrier`` and ``min_delay``.
  ``layer_barrier`` turns off the barrier between all qubits in the layer,
  so that layer run becomes simultaneous direct RB. ``min_delay`` allows the
  user to enforce a minimum delay during the two-qubit layer using a qubit
  that is not part of the two-qubit gate list. Also added the EPL
  (error per layer) calculation to the layer analysis output and to the figure.


.. _Release Notes_0.10.0_Bug Fixes:

Bug Fixes
---------

.. releasenotes/notes/fine-freq-basis-gates-0c8d6a6f895b0ed5.yaml @ b'a4001c2dd407c11ed648df529c8f99751b5966fb'

- The :class:`.FineFrequency` experiment was modified so that it no longer sets
  the ``basis_gates`` in its transpile options to default to the gates used
  in its ``circuits()`` method. If these gates were already in the backend's
  target, there should be no change in behavior. If the gates were not in the
  backend's target, then the experiment would have been producing transpiled
  circuits that did not match the backend's instruction set and so would not
  run. The ``basis_gates`` setting was a remnant from the previous support
  for pulse calibrations for which the calibration experient would use the
  ``basis_gates`` option to know which pulse gate calibrations to attach to
  the circuits.

.. releasenotes/notes/lf-1q-gates-6402f24811d97d5e.yaml @ b'388dec711df9f3182e922199f9b2c4072aaa35f6'

- :class:`.LayerFidelity` now filters out non-standard one qubit gates from
  the target when choosing the set to use for transpiling the one qubit
  Clifford gates. Previously, it would use all one qubit gates in the
  backend's target. With Qiskit 2.0, transpiling with a non-standard gate in
  the way that :class:`.LayerFidelity` does leads to an exception. A warning
  is emitted if a non-standard one qubit gate is filtered out.


.. _Release Notes_0.9.0:

0.9.0
=====

.. _Release Notes_0.9.0_Prelude:

Prelude
-------

.. releasenotes/notes/prepare-0.9.0-282a17f0a3007d62.yaml @ b'fd279872e9236539ab15df16077a0a764e4bdf66'

The Qiskit Experiments 0.9.0 release primarily provides compatibility with
version 2.0 of the `Qiskit SDK <https://www.ibm.com/quantum/qiskit>`__. A major
change is the removal the previously deprecated support for pulse experiments
and calibration due to the removal of Qiskit Pulse in Qiskit 2.0. Some other
smaller changes were also needed to adapt to Qiskit 2.0 as described below.
Additionally, previously deprecated code related curve fitting and restless
measurements was removed. Some additional code paths have been marked as
deprecated, particularly relating to :meth:`.ExperimentData.analysis_results`
as described below.


.. _Release Notes_0.9.0_New Features:

New Features
------------

.. releasenotes/notes/deprecate-old-curvefit-8f532de113c184c4.yaml @ b'fd279872e9236539ab15df16077a0a764e4bdf66'

- Curve analysis helper functions :class:`.utils.inverse_weighted_variance`,
  :class:`.utils.sample_weighted_average`, and
  :class:`.utils.shot_weighted_average` were documented as public. These
  functions were present in previous releases but not documented as public.

.. releasenotes/notes/qiskit2-removals-ee52d83a3eaffd06.yaml @ b'fd279872e9236539ab15df16077a0a764e4bdf66'

- Classes :class:`.MeasLevel` and :class:`.MeasReturnType` were added to
  enumerate the supported measurement levels (kerneled or classified) and
  return types (averaged or not) for the results of jobs run by experiments.
  Previously, private classes of Qiskit were inadvertently used for these
  purposes but they were removed in Qiskit 2.0.


.. _Release Notes_0.9.0_Upgrade Notes:

Upgrade Notes
-------------

.. releasenotes/notes/analysis-results-deprecations-bb97e067d18a2a08.yaml @ b'fd279872e9236539ab15df16077a0a764e4bdf66'

- To aid with the deprecation of the ``results`` argument to
  :meth:`.ExperimentData.add_analysis_results` (see Deprecation Notes),
  :meth:`.AnalysisResultData.as_table_element` was added to
  :class:`.AnalysisResultData`. The new method allows for a result to be
  passed directly to :meth:`.ExperimentData.add_analysis_results` through
  Python dictionary unpacking like
  ``expdata.add_analysis_result(**result.as_table_element())``.

.. releasenotes/notes/analysis-results-deprecations-bb97e067d18a2a08.yaml @ b'fd279872e9236539ab15df16077a0a764e4bdf66'

- The way that warnings about deprecated arguments are issued has been
  updated to be more reliable. Previously, warnings would not be issued for
  deprecated arguments that were left with their default values or that were
  used in internal calls by other functions in the package. The result of the
  old behavior was mainly that arguments to
  :meth:`.ExperimentData.add_analysis_results` and
  :meth:`.ExperimentData.analysis_results` that should have previously been
  issuing pending deprecation warnings did not. This release still proceeds
  with upgrading those pending deprecations to full deprecations.

.. releasenotes/notes/analysis-results-deprecations-bb97e067d18a2a08.yaml @ b'fd279872e9236539ab15df16077a0a764e4bdf66'

- :class:`.RBAnalysis` has been updated so that it accepts for the
  ``epg_1_qubit`` option the form of analysis results returned by
  :meth:`.ExperimentData.analysis_results` when passing ``dataframe=True``.

.. releasenotes/notes/qiskit2-removals-ee52d83a3eaffd06.yaml @ b'fd279872e9236539ab15df16077a0a764e4bdf66'

- In preparation for Qiskit 2.0, references to ``BackendV1`` have been
  guarded so that they do not cause import errors. ``BackendV1`` is still
  supported for now, but support will be removed in a future release.

.. releasenotes/notes/qiskit2-removals-ee52d83a3eaffd06.yaml @ b'fd279872e9236539ab15df16077a0a764e4bdf66'

- :class:`.LocalReadouMitigator` now provides a more specific warning if it
  is used with a backend without a ``properties()`` method or with a backend
  that does not the require readout error properties.

.. releasenotes/notes/qiskit2-removals-ee52d83a3eaffd06.yaml @ b'fd279872e9236539ab15df16077a0a764e4bdf66'

- :class:`.ExperimentData` was updated to handle changes to
  :class:`qiskit.result.Result` in Qiskit 2.0. With Qiskit 2.0, the result
  header is a dict instead of a custom class. Qiskit Experiments uses this
  header to store metadata about experiments' circuits and now handles both
  the dict and class forms.

.. releasenotes/notes/qiskit2-removals-ee52d83a3eaffd06.yaml @ b'fd279872e9236539ab15df16077a0a764e4bdf66'

- A performance issue in :class:`.QuantumVolume` was addressed. When Qiskit
  Aer is installed, :class:`.QuantumVolume` uses
  :class:`qiskit_aer.AerSimulator` to simulate the outcomes of the quantum
  volume circuits to determine the heavy output probabilities. In its default
  instantiation, ``AerSimulator`` regenerates its
  :class:`qiskit.transpiler.Target` every time it is accessed and this
  generation takes an appreciable amount of time. Because the target was
  being accessed separately for every circuit, this overhead could accumulate
  to over a minute in the standard 100 circuit experiment. This overhead was
  reduced by processing all the circuits in one pass.

.. releasenotes/notes/remove-pulse-deprecations-1abbc72dd1376ec2.yaml @ b'fd279872e9236539ab15df16077a0a764e4bdf66'

- Previously deprecated experiments involving pulse gate calibrations have
  been removed, due to the upcoming `removal of Qiskit Pulse in Qiskit
  2.0 <https://github.com/Qiskit/qiskit/issues/13063>`_. These experiments
  include ``QubitSpectroscopy``, ``EFSpectroscopy``, ``Rabi``, ``EFRabi``,
  ``ResonatorSpectroscopy``, ``RoughDrag``, ``StarkRamseyXY``,
  ``StarkRamseyXYAmpScan``, ``StarkP1Spectroscopy``,
  ``CrossResonanceHamiltonian``, ``EchoedCrossResonanceHamiltonian``,
  ``BaseCalibrationExperiment``,
  ``RoughFrequencyCal``, ``RoughEFFrequencyCal``, ``FrequencyCal``,
  ``FineFrequencyCal``, ``RoughDragCal``, ``FineXDragCal``,
  ``FineSXDragCal``, ``FineDragCal``, ``FineAmplitudeCal``,
  ``FineXAmplitudeCal``, ``FineSXAmplitudeCal``, ``HalfAngleCal``,
  ``RoughAmplitudeCal``, ``RoughXSXAmplitudeCal``, and
  ``EFRoughXSXAmplitudeCal``. The associated analysis and helper classes like
  ``ResonanceAnalysis``, ``CrossResonanceHamiltonianAnalysis``,
  ``DragCalAnalysis``, ``ResonatorSpectroscopyAnalysis``, and
  ``StarkCoefficients`` were also removed.

.. releasenotes/notes/remove-pulse-deprecations-1abbc72dd1376ec2.yaml @ b'fd279872e9236539ab15df16077a0a764e4bdf66'

- Also due to the deprecation of Qiskit Pulse, support for providing pulse
  gate calibrations to excite higher levels has been removed from
  :class:`.MultiStateDiscrimination`.

.. releasenotes/notes/remove-pulse-deprecations-1abbc72dd1376ec2.yaml @ b'fd279872e9236539ab15df16077a0a764e4bdf66'

- The ``Calibrations`` class and all of Qiskit Experiments' calibration
  support (including gate libraries and calibration updaters and experiments)
  have been removed. The calibrations features were based on adjusting
  parameters of pulses used in gates. With the removal of pulse support in
  Qiskit 2.0, it no longer was feasible to keep support for features that
  rely on pulse gate  calibrations.

.. releasenotes/notes/remove-pulse-deprecations-1abbc72dd1376ec2.yaml @ b'fd279872e9236539ab15df16077a0a764e4bdf66'

- The methods of :class:`.BackendData` and :class:`.BackendTiming` that
  involved pulse gate features have been removed. The removed methods were
  ``control_channel``, ``drive_channel``, ``measure_channel``,
  ``acquire_channel``, ``min_length``, ``pulse_alignment``,
  ``acquire_alignment``, ``drive_freqs``,  and ``meas_freqs`` of
  ``BackendData`` and ``round_pulse`` and ``pulse_time`` of
  ``BackendTiming``.

.. releasenotes/notes/remove-pulse-deprecations-1abbc72dd1376ec2.yaml @ b'fd279872e9236539ab15df16077a0a764e4bdf66'

- The ability for :class:`.ExperimentEncoder` and :class:`.ExperimentDecoder`
  to work with the pulse ``ScheduleBlock`` class has been removed.

.. releasenotes/notes/remove-pulse-deprecations-1abbc72dd1376ec2.yaml @ b'fd279872e9236539ab15df16077a0a764e4bdf66'

- ``qiskit-dynamics`` has been removed from the ``extras`` extra of the
  package. This means that ``pip install qiskit-experiments[extras]`` will no
  longer install ``qiskit-dynamics``. :class:`.PulseBackend` and
  :class:`.SingleTransmonTestBackend` still require ``qiskit-dynamics`` to be
  installed, so it must be installed separately. The reason for this change
  is that Qiskit Experiments aims to keep compatibility with the latest
  Qiskit and so will not require a package that pins the Qiskit version while
  Qiskit Dynamics plans to pin to Qiskit version 1.

.. releasenotes/notes/remove-restless-43e2c683bd7a4d1d.yaml @ b'167b51283f3c700def4c4cbe4f93f8f2cf1c5f48'

- The previously deprecated support for running experiments with restless
  measurements has been removed. The ``RestlessMixin`` class has been removed
  from the package and thus has also been removed from classes like
  :class:`.RamseyXY` and :class:`.StandardRB` on which it was previously
  applied.

.. releasenotes/notes/remove-return-fit-parameters-1c96a70a1bcf99a1.yaml @ b'a9a1ff5a191d8647cece73473d2f71faca46979c'

- The deprecated analysis option ``return_fit_parameters`` has been removed
  from :class:`.CurveAnalysis` and :class:`.CompositeCurveAnalysis`. This
  change means that the fit parameter analysis result that started with
  ``@Parameters`` will no longer be included in the set of analysis results.
  Code calling :meth:`.ExperimentData.analysis_results` with a numerical
  index, rather than a result name or using ``dataframe=True`` (the
  recommended pattern) may find a different result than it did before. Fit
  parameters should be accessed using :meth:`.ExperimentData.artifacts` to
  retrieve the ``fit_parameters`` artifact.


.. _Release Notes_0.9.0_Deprecation Notes:

Deprecation Notes
-----------------

.. releasenotes/notes/analysis-results-deprecations-bb97e067d18a2a08.yaml @ b'fd279872e9236539ab15df16077a0a764e4bdf66'

- The ``results`` argument to :meth:`.ExperimentData.add_analysis_results`,
  which had previously been marked as pending deprecation, has now been
  marked as deprecated. The preferred form going forward is to create
  analysis results by passing individual properties directly as keyword
  arguments to the method rather than creating separate analysis result
  objects and passing them to the method. To reflect the fact that a single
  result is added per call in this way, the method
  :meth:`.ExperimentData.add_analysis_result` has been added as an
  alternative to :meth:`.ExperimentData.add_analysis_results`.

.. releasenotes/notes/analysis-results-deprecations-bb97e067d18a2a08.yaml @ b'fd279872e9236539ab15df16077a0a764e4bdf66'

- Passing ``False`` to :meth:`.ExperimentData.analysis_results` for the
  ``dataframe`` argument has been deprecated. Previously, passing
  ``False`` had been marked as pending deprecation. To preserve backwards
  compatibility, the default value of ``dataframe`` remains ``False`` but a
  future release could change the default to ``True``.  Because this is a
  major change to the interface, it is not planned that the default will
  change in the next three months (the shortest allowed deprecation cycle).

  A previously common pattern was to retrieve a single result by name from an
  :class:`.ExperimentData` object ``expdata`` like:

  .. code-block:: python

     result = expdata.analysis_results("T1")
     print(f"T1 is {result.value}")

  which makes use of implicit returning the result directly instead of a list
  when there is only one match. With ``dataframe=True``, the dataframe is not
  implicitly truncated so the first match must be selected.
  :attr:`pandas.DataFrame.iloc` can be used to select the first match. The
  following block is equivalent to the previous example:

  .. code-block:: python

     result = expdata.analysis_results("T1", dataframe=True)
     print(f"T1 is {result.iloc[0].value}")

.. releasenotes/notes/deprecate-old-curvefit-8f532de113c184c4.yaml @ b'fd279872e9236539ab15df16077a0a764e4bdf66'

- Curve analysis utility functions
  :func:`.filter_data`,
  :func:`.mean_xy_data`, :func:`.multi_mean_xy_data`, and
  :func:`.data_sort` have been
  deprecated. These methods were written to work with the previous
  representation of curve data. Curve analysis now works with
  :class:`.ScatterTable` which provides a ``filter`` method which can be used
  with functions like :func:`.shot_weighted_average` to achieve similar
  results to the deprecated functions.

.. releasenotes/notes/deprecate-old-curvefit-8f532de113c184c4.yaml @ b'fd279872e9236539ab15df16077a0a764e4bdf66'

- The :class:`.ScatterTable` properties ``data_allocation`` and
  ``labels`` and method ``get_subset_of`` have been deprecated.
  ``data_allocation`` was renamed to ``series_id``. ``labels`` can be found
  by looking at the ``series_name`` of the scatter table's ``dataframe``.
  Data subsets can be obtained using :meth:`.ScatterTable.filter` in place of
  ``get_subset_of``.

.. releasenotes/notes/qiskit2-removals-ee52d83a3eaffd06.yaml @ b'fd279872e9236539ab15df16077a0a764e4bdf66'

- Support for using ``BackendV1`` with Qiskit Experiments is deprecated. Some
  code paths will generate warnings when using ``BackendV1`` but not all
  cases are checked. Support is planned to be removed along all support for
  Qiskit 1 in a future release near the end of support for Qiskit 1.4.

.. releasenotes/notes/remove-pulse-deprecations-1abbc72dd1376ec2.yaml @ b'fd279872e9236539ab15df16077a0a764e4bdf66'

- :class:`.FineZXAmplitude` has been deprecated. It was not deprecated along
  with the other pulse-related experiments in the previous Qiskit Experiments
  release, but it requires an ``ZX`` rotation of :math:`\pi / 2` which is not
  easily realizable on any known providers without pulse calibrations.

.. releasenotes/notes/remove-pulse-deprecations-1abbc72dd1376ec2.yaml @ b'fd279872e9236539ab15df16077a0a764e4bdf66'

- :class:`.PulseBackend` and :class:`.SingleTransmonTestBackend` have been
  deprecated. These backend classes use `Qiskit Dynamics
  <https://github.com/qiskit-community/qiskit-dynamics>`_ to simulate quantum
  circuits using Qiskit Pulse with pulse-level simulation and were mainly
  intended for testing purposes. With the removal of pulse features in Qiskit
  2.0, these classes can no longer be maintained. Their deprecation was
  missed in the previous round of pulse feature deprecation. While the
  classes are kept for one more release cycle, they require Qiskit less than
  2.0 in order to function.

.. releasenotes/notes/remove-restless-43e2c683bd7a4d1d.yaml @ b'167b51283f3c700def4c4cbe4f93f8f2cf1c5f48'

- The data processing nodes :class:`.RestlessNode`,
  :class:`.RestlessToCounts`, and :class:`.RestlessToIQ` have been
  deprecated. These deprecations follow on the removal of ``RestlessMixin``
  and complete the deprecation of restless measurement code from
  qiskit-experiments. In principle, users may copy the code of these
  deprecated nodes and use it to create a custom restless data processor for
  use on experiments even without support in the base Qiskit Experiments
  package.


.. _Release Notes_0.9.0_API Changes for Experiment Authors:

API Changes for Experiment Authors
----------------------------------

.. releasenotes/notes/pyproject-016b2b578cf04a49.yaml @ b'fd279872e9236539ab15df16077a0a764e4bdf66'

- The Python package configuration has been moved from ``setup.py`` to
  ``pyproject.toml``. As part of this move, the separate requirements files
  have also been moved into ``pyproject.toml``. See the `CONTRIBUTING.md file
  <https://github.com/qiskit-community/qiskit-experiments/blob/main/CONTRIBUTING.md>`_
  in the `source repository
  <https://github.com/qiskit-community/qiskit-experiments>`__ for information
  on setting up a development environment if not using ``tox`` (which should
  continue working the same way).


.. _Release Notes_0.8.2:

0.8.2
=====

.. _Release Notes_0.8.2_Prelude:

Prelude
-------

.. releasenotes/notes/prelude-0.8.2-da9fc8763bcd5d4c.yaml @ b'f0d4a782ff0775e6dc409895c955707ff8819f5f'

Qiskit Experiments 0.8.2 is a minor update to 0.8.1 to fix an issue with tracking the job status of Qiskit IBM Runtime jobs.


.. _Release Notes_0.8.2_Bug Fixes:

Bug Fixes
---------

.. releasenotes/notes/fix-jobv2-status-4cf3443e2f3a16b9.yaml @ b'f0d4a782ff0775e6dc409895c955707ff8819f5f'

- This release contains a single fix for :class:`~.ExperimentData` job status handling.


.. _Release Notes_0.8.2_Experiment Data Fixes:

Experiment Data Fixes
^^^^^^^^^^^^^^^^^^^^^

.. releasenotes/notes/fix-jobv2-status-4cf3443e2f3a16b9.yaml @ b'f0d4a782ff0775e6dc409895c955707ff8819f5f'

- Fixed :class:`~.ExperimentData` not reporting the correct job status when
  the job is of type :class:`~qiskit_ibm_runtime.RuntimeJobV2`. See `#1512
  <https://github.com/qiskit-community/qiskit-experiments/pull/1512>`__.


.. _Release Notes_0.8.2_API Changes for Experiment Authors:

API Changes for Experiment Authors
----------------------------------

.. releasenotes/notes/fix-jobv2-status-4cf3443e2f3a16b9.yaml @ b'f0d4a782ff0775e6dc409895c955707ff8819f5f'

- Added a function that returns the job status as :class:`qiskit.providers.jobstatus.JobStatus`:
  :meth:`~qiskit_experiments.framework.experiment_data.get_job_status`.


.. _Release Notes_0.8.1:

0.8.1
=====

.. _Release Notes_0.8.1_Prelude:

Prelude
-------

.. releasenotes/notes/prelude-0.8.1-d98e520584b5d73e.yaml @ b'aecea7b3e6ee565feb904275fb1142d221307c01'

Qiskit Experiments 0.8.1 is a minor update to 0.8.0 to improve compatibility with dependencies Qiskit and CVXPY. References to code in these dependencies were updated to avoid triggering deprecation warnings, so that the code will continue to work as new versions of Qiskit and CVXPY are released. Additional examples were added to the documentation, and some typos were corrected.


.. _Release Notes_0.8.1_New Features:

New Features
------------

.. releasenotes/notes/adopt-mitigators-d7ad0e2f3cd2fa57.yaml @ b'd2b41d69fdce990b7829f392012878c926ef41d4'

- New :class:`~.LocalReadoutMitigator` and
  :class:`~.CorrelatedReadoutMitigator` classes have been added. These
  classes were moved directly from Qiskit which deprecated them in Qiskit
  1.3. They provide utility methods for applying readout error mitigation and
  integrate with the readout error mitigation experiments
  :class:`~.LocalReadoutError` and :class:`~.CorrelatedReadoutError`.


.. _Release Notes_0.8.1_Upgrade Notes:

Upgrade Notes
-------------

.. releasenotes/notes/adopt-mitigators-d7ad0e2f3cd2fa57.yaml @ b'd2b41d69fdce990b7829f392012878c926ef41d4'

- The readout error mitigation experiments :class:`~.LocalReadoutError` and
  :class:`~.CorrelatedReadoutError` have been updated to generate instances
  of the new :class:`~.LocalReadoutMitigator` and
  :class:`~.CorrelatedReadoutMitigator` classes. The experiments should
  continue to work as before, but any code that was using, for example,
  ``isinstance()`` to check object type would need to be updated to check
  against the Qiskit Experiments classes instead of the old Qiskit classes.

.. releasenotes/notes/qiskit13-deprecations-afece0ceea29f3f7.yaml @ b'aecea7b3e6ee565feb904275fb1142d221307c01'

- Minor adjustments were made to Qiskit Experiments internals to avoid
  deprecation warnings when using Qiskit 1.3. See `#1482
  <https://github.com/qiskit-community/qiskit-experiments/pull/1482>`__ and
  `#1491 <https://github.com/qiskit-community/qiskit-experiments/pull/1491>`__.


.. _Release Notes_0.8.1_Bug Fixes:

Bug Fixes
---------

.. releasenotes/notes/provider-service-types-47c262d5434047e5.yaml @ b'a0429edfeecb6292de05001aba2ad293c0067262'

- Fixed  :class:`~.ExperimentData` not inferring the  credentials for the IBM
  experiment service from a :class:`~qiskit_ibm_runtime.QiskitRuntimeService`
  instance as it used to do for ``qiskit-ibm-provider``. Previously, the IBM
  experiment service was set up in the :class:`~.ExperimentData` constructor,
  but now it is done on first attempt to use the service, allowing more time
  for the service to be set explicitly or for other attributes to be set that
  help with inferring the credentials.

.. releasenotes/notes/provider-service-types-47c262d5434047e5.yaml @ b'a0429edfeecb6292de05001aba2ad293c0067262'

- Fixed a bug where :meth:`.ExperimentData.add_data` would not work when
  passed a single :class:`qiskit.primitives.PrimitiveResult` object.


.. _Release Notes_0.8.1_API Changes for Experiment Authors:

API Changes for Experiment Authors
----------------------------------

.. releasenotes/notes/provider-service-types-47c262d5434047e5.yaml @ b'a0429edfeecb6292de05001aba2ad293c0067262'

- Added classes
  :class:`~qiskit_experiments.framework.BaseJob`,
  :class:`~qiskit_experiments.framework.ExtendedJob`,
  :class:`~qiskit_experiments.framework.Job`,
  :class:`~qiskit_experiments.framework.BaseProvider`,
  :class:`~qiskit_experiments.framework.IBMProvider`, and
  :class:`~qiskit_experiments.framework.Provider` to document the interfaces
  needed by :class:`~.ExperimentData` to work with jobs and results.


.. _Release Notes_0.8.1_Other Notes:

Other Notes
-----------

.. releasenotes/notes/add-examples-to-experiments-api-docs-31f3e3c3369a6f3d.yaml @ b'1bb808910166276e9e857fe9ec862d500efae8c9'

- Added minimal working code examples to the API pages for the experiments,
  :class:`~.EFSpectroscopy`,
  :class:`~.EFRabi`,
  :class:`~.FineAmplitudeCal`,
  :class:`~.FineXAmplitudeCal`,
  :class:`~.FineSXAmplitudeCal`,
  :class:`~.FineDragCal`,
  :class:`~.FineXDragCal`,
  :class:`~.FineSXDragCal`,
  :class:`~.FineFrequencyCal`,
  :class:`~.FrequencyCal`,
  :class:`~.HalfAngleCal`,
  :class:`~.RoughAmplitudeCal`,
  :class:`~.RoughXSXAmplitudeCal`,
  :class:`~.EFRoughXSXAmplitudeCal`,
  :class:`~.RoughDragCal`,
  :class:`~.RoughFrequencyCal`,
  :class:`~.QuantumVolume`,
  :class:`~.InterleavedRB`,
  :class:`~.LayerFidelity`,
  :class:`~.StandardRB`,
  :class:`~.MitigatedProcessTomography`,
  :class:`~.MitigatedStateTomography`,
  :class:`~.ProcessTomography` and
  :class:`~.StateTomography`.
  The backends used in the code examples are simulators
  such as, ``SingleTransmonTestBackend()`` and
  ``AerSimulator(FakePerth())``.


.. _Release Notes_0.8.0:

0.8.0
=====

.. _Release Notes_0.8.0_Prelude:

Prelude
-------

.. releasenotes/notes/0.8/primitives_add-1a3bcbb2f189d18e.yaml @ b'182a6aee495cf3456fd1f14e34af05881c96b4e6'

In this release we added support for the Qiskit primitives. Qiskit Experiments will execute circuits using :class:`qiskit_ibm_runtime.SamplerV2` by default.


.. _Release Notes_0.8.0_Upgrade Notes:

Upgrade Notes
-------------

.. releasenotes/notes/0.8/primitives_add-1a3bcbb2f189d18e.yaml @ b'182a6aee495cf3456fd1f14e34af05881c96b4e6'

- When only a ``backend`` is set on an experiment, :meth:`qiskit_experiments.framework.BaseExperiment.run`
  now defaults to wrapping the ``backend`` in a :class:`qiskit_ibm_runtime.SamplerV2` and
  using that to execute the circuits. A new ``sampler`` argument is also
  accepted by ``run()`` to allow for a custom :class:`qiskit.primitives.BaseSamplerV2`
  instance to be used for circuit execution. ``run()`` also accepts a ``backend_run``
  option which will cause the old ``backend.run`` path to be used for circuit execution.
  However, the ``backend.run()`` method is scheduled to be removed from
  qiskit-ibm-runtime in the near future.


.. _Release Notes_0.8.0_Deprecation Notes:

Deprecation Notes
-----------------

.. releasenotes/notes/0.8/deprecate-pulse-2a13fc783985ac27.yaml @ b'182a6aee495cf3456fd1f14e34af05881c96b4e6'

- Experiments involving pulse gate calibrations have been deprecated, due to
  the upcoming `deprecation of Qiskit Pulse in Qiskit 2.0
  <https://github.com/Qiskit/qiskit/issues/13063>`_. These experiments
  include ``QubitSpectroscopy``, ``EFSpectroscopy``, ``Rabi``, ``EFRabi``,
  ``ResonatorSpectroscopy``, ``RoughDrag``, ``StarkRamseyXY``,
  ``StarkRamseyXYAmpScan``, ``StarkP1Spectroscopy``,
  ``CrossResonanceHamiltonian``, ``EchoedCrossResonanceHamiltonian``,
  ``RoughFrequencyCal``, ``RoughEFFrequencyCal``, ``FrequencyCal``,
  ``FineFrequencyCal``, ``RoughDragCal``, ``FineXDragCal``,
  ``FineSXDragCal``, ``FineDragCal``, ``FineAmplitudeCal``,
  ``FineXAmplitudeCal``, ``FineSXAmplitudeCal``, ``HalfAngleCal``,
  ``RoughAmplitudeCal``, ``RoughXSXAmplitudeCal``, and
  ``EFRoughXSXAmplitudeCal``.

.. releasenotes/notes/0.8/deprecate-pulse-2a13fc783985ac27.yaml @ b'182a6aee495cf3456fd1f14e34af05881c96b4e6'

- Also due to the deprecation of Qiskit Pulse, support for providing pulse
  gate calibrations to excite higher levels has been deprecated from
  :class:`.MultiStateDiscrimination`.

.. releasenotes/notes/0.8/deprecate-pulse-2a13fc783985ac27.yaml @ b'182a6aee495cf3456fd1f14e34af05881c96b4e6'

- The ``Calibrations`` class and all of Qiskit Experiments' calibration
  support is deprecated. The calibrations features were based on adjusting
  parameters of pulses used in gates. With the deprecation of Qiskit Pulse,
  these features are now also deprecated.

.. releasenotes/notes/0.8/deprecate-pulse-2a13fc783985ac27.yaml @ b'182a6aee495cf3456fd1f14e34af05881c96b4e6'

- Support for running experiments in restless mode using the
  ``RestlessMixin`` is deprecated. With improvements in the reliability of
  IBM Quantum's qubit initialization, circuit exectuion has already become
  reasonably fast and restless measurements do not add much performance
  improvement. It is expected that the restless features are little used as
  there has been no recent user feedback about them.


.. _Release Notes_0.8.0_Bug Fixes:

Bug Fixes
---------

.. releasenotes/notes/0.8/fixes_qiskit_1_2-6d60334235081088.yaml @ b'182a6aee495cf3456fd1f14e34af05881c96b4e6'

- Qiskit Experiments was updated to be compatible with Qiskit 1.2,
  including removing references to ``QuantumCircuit._parameter_table``
  which prevented randomized benchmarking and layer fidelity
  experiments from running.



.. _Release Notes_0.7.0:

0.7.0
=====

.. _Release Notes_0.7.0_Prelude:

Prelude
-------

.. releasenotes/notes/0.7/0_7_release-96efcec2c45dcf74.yaml @ b'517532eb307897896d549f7e3de69485801d67a8'

The Qiskit Experiments 0.7 release adds the Layer Fidelity experiment and makes some fixes and improvements.


.. _Release Notes_0.7.0_New Features:

New Features
------------

.. releasenotes/notes/0.7/layer-fidelity-1e09dea9e5b69515.yaml @ b'517532eb307897896d549f7e3de69485801d67a8'

- Add a new experiment class :class:`.LayerFidelity` to measure
  `layer fidelity and EPLG (error per layered gate) <https://arxiv.org/abs/2311.05933>`_,
  which is a holistic benchmark to characterize the full quality of the devices at scale.

  It has an experimental feature: its :meth:`circuits`
  exceptionally returns circuits on physical qubits (not virtual qubits as usual).
  Its analysis class :class:`.LayerFidelityAnalysis` returns :class:`.AnalysisResultData`
  which contains several ``extra`` entries to help additional analyses: e.g.
  ``qubits`` to ease the query of sub-analysis results and
  ``reason`` to tell users why the ``quality`` of the analysis was ``"bad"``.

  For example, the syntax for pulling out the individual fidelities looks like below.

  .. code-block:: python

    df = exp_data.analysis_results(dataframe=True)
    df[(df.name=="ProcessFidelity") & (df.qubits==(59, 60))].value

  See `an example notebook
  <https://github.com/qiskit-community/qiskit-device-benchmarking/blob/main/notebooks/layer_fidelity.ipynb>`_
  for more examples such as how to select a best possible qubit chain to measure and
  how to plot EPLG as a function of (sub)chain length.

.. releasenotes/notes/0.7/residuals_plot-377aabb9193a5a98.yaml @ b'517532eb307897896d549f7e3de69485801d67a8'

- Added an option to plot residuals for single-figure experiments, which is enable by setting ``plot_residuals`` to ``True``.

.. releasenotes/notes/0.7/residuals_plot-377aabb9193a5a98.yaml @ b'517532eb307897896d549f7e3de69485801d67a8'

- Introduced ``sub_plot_heights_list`` and ``sub_plot_widths_list`` attributes in :class:`.PlotStyle` for customizable
  subplot sizes where each list needs to sum up to 1. This feature currently works only for experiments with
  no subplots.


.. _Release Notes_0.7.0_Known Issues:

Known Issues
------------

.. releasenotes/notes/0.7/0_6_deprecations-9a399c48c2d461f1.yaml @ b'517532eb307897896d549f7e3de69485801d67a8'

- Fit parameters are not populated in composite curve analysis results and are
  found only in the fit summary artifact. In a future release, they will be
  removed from all analysis result objects and live in the artifacts only.


.. _Release Notes_0.7.0_Upgrade Notes:

Upgrade Notes
-------------

.. releasenotes/notes/0.7/0_7_release-96efcec2c45dcf74.yaml @ b'517532eb307897896d549f7e3de69485801d67a8'

- Changes in behavior that users should be aware of when upgrading Qiskit Experiments
  to this version are listed below in subsections by functional area.


.. _Release Notes_0.7.0_Package Upgrades:

Package Upgrades
^^^^^^^^^^^^^^^^

.. releasenotes/notes/0.7/0_6_deprecations-9a399c48c2d461f1.yaml @ b'517532eb307897896d549f7e3de69485801d67a8'

- Several deprecated modules, methods, functions, and options have been removed and will no longer work:

  * The visualization module of :mod:`.CurveAnalysis` has been replaced by the
    standalone :mod:`.visualization` module. The ``LegacyCurveCompatDrawer`` has
    been removed from :mod:`.visualization`.
  * The ``curve_drawer`` option to :class:`.CompositeCurveAnalysis` has been
    replaced by the plotter in the visualization module.
  * The ``SeriesDef`` dataclass has been removed and replaced by the ``LMFIT`` module.
  * The ``CurveData`` dataclass has been removed and replaced by :class:`.ScatterTable`'s DataFrame representation.
  * ``random_cliffords()`` and ``random_clifford_circuits()`` have been
    removed from :class:`.CliffordUtils` and replaced by :meth:`.StandardRB.__sample_sequence`.
  * ``beta`` has been renamed to ``outcome_prior`` in the tomography utility
    function ``binomial_weights()``.
  * The ``return_data_points`` option has been removed from curve analysis.
    Data points are now automatically provided in :class:`.ExperimentData` objects via the ``curve_data``
    artifact.
  * The default value of ``flatten_results`` in composite experiments has changed to ``True``.


.. _Release Notes_0.7.0_Deprecation Notes:

Deprecation Notes
-----------------

.. releasenotes/notes/0.7/0_6_deprecations-9a399c48c2d461f1.yaml @ b'517532eb307897896d549f7e3de69485801d67a8'

- Accessing experiment data artifacts by numerical index has been
  deprecated. Use the name or ID of the artifact instead.


.. _Release Notes_0.7.0_Bug Fixes:

Bug Fixes
---------

.. releasenotes/notes/0.7/fix-ecr-epg-59c8db98494966b0.yaml @ b'7a0924c24549ab4f38819a86c0ac49214d819ba2'

- Fixed a bug in EPG (error per gate) computation in :class:`~.RBAnalysis`
  where it fails with a ``TypeError`` for backends with ECR gate
  as a 2-qubit basis gate (e.g. IBM Eagle processors).
  See
  `#1419
  <https://github.com/Qiskit-Community/qiskit-experiments/pull/1419>`_.
  for the details of the bug.

.. releasenotes/notes/0.7/fix-epg-gatecount-60777f7a3f3566bc.yaml @ b'517532eb307897896d549f7e3de69485801d67a8'

- The gate counting for EPG in the RB analysis code was not including the
  inverse, so that the total number of operations per Clifford was incorrect,
  leading to incorrect reporting of EPG from EPC. Fixed by adding +1 for the
  inverse gate.


.. _Release Notes_0.7.0_Other Notes:

Other Notes
-----------

.. releasenotes/notes/0.7/add-examples-to-characterization-experiments-e77d4d26c6b49694.yaml @ b'517532eb307897896d549f7e3de69485801d67a8'

- Added minimal working code examples to many experiment API pages,
  especially characterization experiments. The minimal working code example
  is a code snippet which users can copy and paste to run the experiment.
  Users are required to specify a backend to use the code example. By default,
  the backend used in the examples is a simulator such as ``FakeManilaV2``.

.. _Release Notes_0.6.1:

0.6.1
=====

.. _Release Notes_0.6.1_Prelude:

Prelude
-------

.. releasenotes/notes/0.6/0_6_1_release-9ccfd5dba7190c77.yaml @ b'5c6f4b2c8226bca2276c5eecfab5193748a8e524'

Qiskit Experiments 0.6.1 is a minor bug fixes release.

.. _Release Notes_0.6.1_Bug Fixes:

Bug Fixes
---------

.. releasenotes/notes/dynamics-0.5-0da56d1ef7d93e77.yaml @ b'1b416add73d70c58dfbd87042d7b75775305717a'

- :class:`.SingleTransmonTestBackend` was updated to be compatible with
  :mod:`qiskit_dynamics` version 0.5.0. The updates accounted for changes in
  the expected arguments to Dynamics API's and did not change behavior. See
  `#1427
  <https://github.com/Qiskit-Community/qiskit-experiments/pull/1427>`__.

.. releasenotes/notes/figure-names-inconsistency-afca1ac8e00fabac.yaml @ b'32813b86b04b956bb7b1334759a79af6ce9068df'

- :class:`.ExperimentData` was updated so that running analysis a second time
  with ``replace_results=True`` does not result in the ``figure_names``
  property having incorrect data (both old and new figure names if the names
  changed). See `#1430
  <https://github.com/Qiskit-Community/qiskit-experiments/pull/1430>`__.

.. releasenotes/notes/figure-names-inconsistency-afca1ac8e00fabac.yaml @ b'32813b86b04b956bb7b1334759a79af6ce9068df'

- :class:`.BaseAnalysis` was updated to respect ``figure_names`` as a keyword
  argument to the ``run()`` method. Previously, this argument was ignored and
  ``figure_names`` could only be set as an analysis option prior to calling
  ``run()``. See `#1430
  <https://github.com/Qiskit-Community/qiskit-experiments/pull/1430>`__.

.. releasenotes/notes/mock-iq-backend-without-qiskit-runtime-20d2bf9edb48312d.yaml @ b'2045689df74b74a94f1f5e5a8d4598354c4e5385'

- :class:`.MockIQBackend` was refactored so that it does not import
  ``qiskit_ibm_runtime`` since
  :external+qiskit_ibm_runtime:doc:`qiskit-ibm-runtime <index>` is not a
  required dependency of Qiskit Experiments.

.. releasenotes/notes/remove_backendv2-b608a2f380698710.yaml @ b'5c6f4b2c8226bca2276c5eecfab5193748a8e524'

- Removed a ``FakeBackendV2`` import path which would have been incompatible with Qiskit 1.1 and above. See
  `#1420 <https://github.com/Qiskit-Community/qiskit-experiments/pull/1420>`_.


.. _Release Notes_0.6.0:

0.6.0
=====

.. _Release Notes_0.6.0_Prelude:

Prelude
-------

.. releasenotes/notes/0.6/0.6_release-4d766733941ad57a.yaml @ b'3b039c5df784748597261d38599c1c7cb2074377'

Qiskit Experiments 0.6 introduces numerous features and improvements. It is
compatible with Qiskit 1.0. Notable changes include: refactoring the analysis
results to a pandas :class:`~pandas:pandas.DataFrame`-based
:class:`.AnalysisResultTable`, the ability to add artifacts of serializable data
to :class:`.ExperimentData`, and refactoring curve fit data into a new
:class:`~pandas:pandas.DataFrame`-based :class:`.ScatterTable` container that is
stored by default as an artifact in :class:`.ExperimentData` along with the
summary of fit results. New experiments include :class:`.StarkP1Spectroscopy`,
:class:`.StarkRamseyXY`, and :class:`.StarkRamseyXYAmpScan`.
:class:`.StandardRB` and :class:`.InterleavedRB` were significantly improved.
The supported provider for running jobs on IBM backends is now
:external+qiskit_ibm_runtime:doc:`qiskit-ibm-runtime <index>`. Using
``qiskit-ibm-provider`` is still supported but its use is deprecated.

.. _Release Notes_0.6.0_New Features:

New Features
------------

.. releasenotes/notes/0.6/0.6_release-4d766733941ad57a.yaml @ b'3b039c5df784748597261d38599c1c7cb2074377'

- New features are listed below in subsections by functional area.


.. _Release Notes_0.6.0_New Experiments:

New Experiments
^^^^^^^^^^^^^^^

.. releasenotes/notes/0.6/mod-stark-1f1afb538a94fe9a.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- New experiment :class:`.StarkRamseyXY` has been added.
  This is a variant of the :class:`.RamseyXY` experiment that characterizes
  the qubit frequency offset under a Stark tone drive.

.. releasenotes/notes/0.6/mod-stark-1f1afb538a94fe9a.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- New experiment :class:`.StarkRamseyXYAmpScan` has been added.
  This is a variant of the :class:`.StarkRamseyXY` experiment to estimate
  the required tone amplitude to cause a particular Stark shift.
  This experiment scans tone amplitude while fixing the Stark tone length,
  and fits the result with the dedicated fitter :class:`.StarkRamseyXYAmpScanAnalysis`.

.. releasenotes/notes/0.6/mod-stark-1f1afb538a94fe9a.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- New experiment :class:`.StarkP1Spectroscopy` has been added.
  This is a variant of :class:`.T1` experiment to conduct spectroscopy of
  qubit relaxation at different qubit frequencies.
  The spectroscopy data is just visualized with the dedicated analysis
  :class:`.StarkP1SpectAnalysis`. A developer may subclass this analysis class to
  perform custom analysis on the spectroscopy data.


.. _Release Notes_0.6.0_Experiment Library Features:

Experiment Library Features
^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. releasenotes/notes/0.6/plugable-rb-clifford-synthesis-0e66c62fa3088fba.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- Added a new experiment option ``clifford_synthesis_method`` to RB experiments,
  e.g. :class:`~.StandardRB` and :class:`~.InterleavedRB` so that users can
  plug in a custom Clifford synthesis algorithm used for generating RB circuits.
  Such a plugin should be implemented as a :class:`~.qiskit.transpiler.passes.synthesis.plugin.HighLevelSynthesisPlugin`
  (see :class:`~.RBDefaultCliffordSynthesis` for example).

.. releasenotes/notes/0.6/plugable-rb-clifford-synthesis-0e66c62fa3088fba.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- Updated :class:`~.InterleavedRB` so that it only accepts ``interleaved_element``
  consisting only of instructions supported by the backend of interest.


.. _Release Notes_0.6.0_Experiment Class Features:

Experiment Class Features
^^^^^^^^^^^^^^^^^^^^^^^^^

.. releasenotes/notes/0.6/circuit-count-method-a095bd74aaa1d2fb.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- A new method :meth:`.BaseExperiment.job_info` has been added that will
  output the number of jobs the experiment is expected to be split into
  based on the provided backend. Refer to issue
  `#1247 <https://github.com/Qiskit-Community/qiskit-experiments/issues/1247>`_
  for more details.

.. releasenotes/notes/0.6/setter-methods-for-experiment-099074e59faffb49.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- Added ``experiment_type`` as optional ``__init__`` kwarg in :class:`.BatchExperiment`
  and :class:`.ParallelExperiment`.

.. releasenotes/notes/0.6/setter-methods-for-experiment-099074e59faffb49.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- ``experiment_type`` can now be easily set and retrieved from the experiment
  object post-construction using the ``experiment_type`` property and setter.


.. _Release Notes_0.6.0_Analysis Class Features:

Analysis Class Features
^^^^^^^^^^^^^^^^^^^^^^^

.. releasenotes/notes/0.6/broadcasting-option-8a3b72bfc1df9668.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- Added a ``broadcast`` option to :class:`.CompositeAnalysis`. When ``broadcast=True`` is passed,
  this option will be applied to child experiment analyses within the class. This means it will iterate
  through the child analysis classes and apply the given option to each of
  them.

.. releasenotes/notes/0.6/selective-figure-generation-0864216f34d3486f.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- The ``generate_figures`` parameter has been added to :class:`.CompositeAnalysis` to control figure
  generation. By default, ``generate_figures`` is ``always``, meaning figures will always be generated.
  If ``generate_figures`` is set to ``selective``, then only figures for analysis results of bad
  quality will be generated. If ``generate_figures`` is set to ``never``, then figures will never be
  generated. This behavior can be overridden for individual analyses by setting the analysis option
  ``plot`` for :class:`.CurveAnalysis`.


.. _Release Notes_0.6.0_Experiment Data Features:

Experiment Data Features
^^^^^^^^^^^^^^^^^^^^^^^^

.. releasenotes/notes/0.6/access_figure_without_extension-5b7438c19e223d6b.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- Figures in :class:`.ExperimentData` objects can now be accessed without the ``.svg`` extension.

.. releasenotes/notes/0.6/add-dataframe-analysis-results-ec8863e826a70621.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- :class:`.ExperimentData` has been upgraded to store analysis result data in
  a table format with the new inline container :class:`.AnalysisResultTable`.
  In this release, the :meth:`.ExperimentData.analysis_results` method still returns
  a conventional list of :class:`.AnalysisResult` for backward compatibility,
  however, when you call the method with new argument ``dataframe=True`` it returns
  analysis results all in one piece with the table format. For example,

  .. code-block:: python

    exp = StandardRB((0,), lengths, backend)
    experiment_data = exp.run().block_for_results()

    experiment_data.analysis_results(dataframe=True, columns="default")

  Information contained in the returned table can be filtered with ``columns`` argument,
  which may take either ``all``, ``default``, ``minimal``, or list of column names.
  Returning a list of :class:`.AnalysisResult` will be deprecated in a future release
  along with the ``dataframe`` option.

  Related to this update, :meth:`.ExperimentData.add_analysis_results` method now takes
  keyword arguments keyed on the table column names, in addition to the argument of
  ``results`` which is either :class:`.AnalysisResult` or a list of it.
  This allows users and developers to bypass creation of :class:`.AnalysisResult` instance
  for registering new entry in the :class:`.ExperimentData` instance.

  Note that the conventional :class:`.AnalysisResult` is originally a payload object for
  saving an analysis result in a remote database, as it implements a REST API
  for the IBM Experiment Service, which is not necessary at all in
  the context of experiment data analysis.
  In a future release, :class:`.AnalysisResult` will be hidden from Qiskit Experiments users.

.. releasenotes/notes/0.6/experiment-artifacts-c481f4e07226ce9e.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- An artifact class has been introduced to store long-form data generated by experiments.
  The :class:`.CurveFitResult` and :class:`.ScatterTable` generated by experiments
  are now stored in artifacts in the :class:`.ExperimentData` class. :meth:`.add_artifacts`
  and :meth:`.delete_artifact` have been added to manipulate the artifacts. These will be uploaded
  to the cloud service in JSON form along with the rest of the :class:`.ExperimentData` object
  when saved. For more information, see the :doc:`artifacts how-to </howtos/artifacts>`.

.. releasenotes/notes/0.6/experiment_data_fixes-f69c3569a8ba1342.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- :meth:`.ExperimentData.save` now uses the multithreading capability
  of the experiment service to enable faster saving times.

.. releasenotes/notes/0.6/experiment_data_fixes-f69c3569a8ba1342.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- :class:`.ExperimentData` now supports the new method
  :meth:`.ExperimentData.get_service_from_provider` enabling the automatic
  setting of the experiment database service via passing the provider.

.. releasenotes/notes/0.6/experiment_data_fixes-f69c3569a8ba1342.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- The ``start_datetime`` property of :class:`.ExperimentData` is now being
  set to the time the experiment data was created.

.. releasenotes/notes/0.6/experiment_data_fixes-f69c3569a8ba1342.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- The ``end_datetime`` property of :class:`.ExperimentData` is now being
  set to the latest time a successful job terminated.

.. releasenotes/notes/0.6/experiment_data_fixes-f69c3569a8ba1342.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- The ``creation_datetime`` and ``updated_datetime`` properties of :class:`.ExperimentData`
  are now being read from the server when saving the experiment.

.. releasenotes/notes/0.6/experiment_data_fixes-f69c3569a8ba1342.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- All the datetime properties are now stored in UTC and converted to local time when using getters.

.. releasenotes/notes/0.6/experiment_data_fixes-f69c3569a8ba1342.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- :meth:`.ExperimentData.save` can now raise exceptions when saving
  fails, unless the ``suppress_errors`` flag is set (on by default).

.. releasenotes/notes/0.6/runtime-provider-support-5358b72ec0035419.yaml @ b'3b039c5df784748597261d38599c1c7cb2074377'

- Experiments run via the :external+qiskit_ibm_runtime:doc:`qiskit-ibm-runtime <index>` provider can now be saved
  to and loaded from the cloud service.


.. _Release Notes_0.6.0_Curve Fit Features:

Curve Fit Features
^^^^^^^^^^^^^^^^^^

.. releasenotes/notes/0.6/add-dataframe-curve-data-a8905c450748b281.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- :class:`.ScatterTable` has been introduced as a drop-in replacement of :class:`.CurveData`.

  This is a data format to store intermediate data in curve analysis built on top of
  the pandas :class:`~pandas:pandas.DataFrame`. Each table row corresponds to a single data point,
  and the table contains all data points generated by the :class:`.CurveAnalysis`.
  All properties and methods of :class:`.CurveData` are implemented for backward compatibility,
  but these will be removed in the future release.

.. releasenotes/notes/0.6/add-dataframe-curve-data-a8905c450748b281.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- New analysis option ``fit_category`` is added to :class:`.CurveAnalysis` subclasses.
  This option controls which data subset within the :class:`.ScatterTable`
  is used for the curve fitting.


.. _Release Notes_0.6.0_Calibration Features:

Calibration Features
^^^^^^^^^^^^^^^^^^^^

.. releasenotes/notes/0.6/feature-support-calibrations-roundtrip-47f09bd9ff803479.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- A JSON data format has been added for saving a :class:`.Calibrations` instance.
  This leverages a custom JSON encoder and decoder to serialize
  the entire calibration data including user provided schedule templates.
  Output JSON data is formatted into the standard data model which is intentionally
  agnostic to the calibration data structure.


.. _Release Notes_0.6.0_Visualization Features:

Visualization Features
^^^^^^^^^^^^^^^^^^^^^^

.. releasenotes/notes/0.6/add-support-for-visualization-with-unshared-axis-9f7bfe272353086b.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- The :class:`.MplDrawer` visualization backend has been upgraded so that
  it can take list of options for ``xlim``, ``ylim``, ``xval_unit``, ``yval_unit``,
  ``xval_unit_scale``, and ``yval_unit_scale``. New figure options
  ``sharex`` and ``sharey`` are also added. The new options are used to unkink the
  configuration of sub axes, and default to ``True`` for backward compatibility.
  By disabling these options, an experiment author can write an analysis class that
  generates a multi-axes figure with different plot ranges.

.. releasenotes/notes/0.6/qvplotter-04efe280aaa9d555.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- An :meth:`~.BaseDrawer.hline` method was added to :class:`.BaseDrawer` for
  generating horizontal lines. See `#1348
  <https://github.com/Qiskit-Community/qiskit-experiments/pull/1348>`__.

.. releasenotes/notes/0.6/qvplotter-04efe280aaa9d555.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- The
  :class:`~qiskit_experiments.library.quantum_volume.QuantumVolumeAnalysis`
  analysis class was updated to use
  :class:`~qiskit_experiments.library.quantum_volume.QuantumVolumePlotter`
  for its figure generation. The appearance of the figure should be the same
  as in previous
  releases, but now it is easier to customize the figure by setting options
  on the plotter object. See `#1348
  <https://github.com/Qiskit-Community/qiskit-experiments/pull/1348>`__.

.. releasenotes/notes/0.6/scale_options-745ee6f8e560043f.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- New figure options were added to the visualization module: ``xscale`` and ``yscale``. They
  represent parameters to the Matplotlib functions ``set_xscale`` and ``set_yscale``: ``log``,
  ``linear``, ``symlog``, ``logit``, and ``quadratic`` (the latter is an additional support
  for quadratic scaling).


.. _Release Notes_0.6.0_Known Issues:

Known Issues
------------

.. releasenotes/notes/0.6/0.6_release-4d766733941ad57a.yaml @ b'3b039c5df784748597261d38599c1c7cb2074377'

- Copied :class:`.ExperimentData` objects don't save their analysis results to the cloud service.
  See `#1396
  <https://github.com/Qiskit-Community/qiskit-experiments/issues/1396>`_.


.. _Release Notes_0.6.0_Upgrade Notes:

Upgrade Notes
-------------

.. releasenotes/notes/0.6/0.6_release-4d766733941ad57a.yaml @ b'3b039c5df784748597261d38599c1c7cb2074377'

- Changes in behavior that users should be aware of when upgrading Qiskit Experiments
  to this version are listed below in subsections by functional area.


.. _Release Notes_0.6.0_Package Upgrades:

Package Upgrades
^^^^^^^^^^^^^^^^

.. releasenotes/notes/0.6/0.5_deprecations-4188ada026cb682b.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- Several deprecated methods and options have been removed and will no longer work:

  * Passing the ``qubits`` keyword argument or an integer qubit index to experiments is no longer
    allowed. Use ``physical_qubits`` keyword argument with a sequence type input.
  * The ``scipy_linear_lstsq`` and ``scipy_gaussian_lstsq`` fitters for the
    :class:`.StateTomographyAnalysis` and :class:`.ProcessTomographyAnalysis`
    classes have been removed. Use the :func:`.cvxpy_linear_lstsq`
    and :func:`.cvxpy_gaussian_lstsq` fitters instead.
  * Curve fit solvers ``curve_fit()`` and ``multi_curve_fit()`` as well as fit functions
    ``bloch_oscillation_x()``, ``bloch_oscillation_y()``, and ``bloch_oscillation_z()`` have been
    removed. Use the LMFIT library instead.
  * The ``flat_top_widths`` argument and experiment option of the
    :class:`.CrossResonanceHamiltonian` experiment and its subclass have been removed. Use
    ``durations`` instead.
  * The ``DumpedOscillationAnalysis`` class has been renamed to :class:`.DampedOscillationAnalysis`.

.. releasenotes/notes/0.6/drop_python_3_7_support-0529a7122e94b004.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- Dropped support for Python 3.7 and added support for Python 3.12.

.. releasenotes/notes/0.6/qiskit-dependency-3f6b8d71cc4d2c31.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- The dependency on ``qiskit-terra`` was replaced with a dependency on
  ``qiskit``. This change follows the move in upstream Qiskit to rename
  ``qiskit-terra`` to ``qiskit``. The minimum required version was increased
  from 0.24 for ``qiskit-terra`` to 0.45 for ``qiskit``. For more information on
  the renaming of Qiskit, see the `Qiskit repository renaming plan
  <https://github.com/Qiskit/RFCs/blob/5793e78dc8e4d8d17f8ef7fad789c6c5ebd3a061/0011-repo-rename.md>`__
  and the `Qiskit 1.0 migration guide <https://quantum.cloud.ibm.com/docs/migration-guides/qiskit-1.0>`__.

.. releasenotes/notes/0.6/runtime-provider-support-5358b72ec0035419.yaml @ b'3b039c5df784748597261d38599c1c7cb2074377'

- With the impending deprecation of the ``qiskit-ibm-provider`` package, support for
  ``qiskit-ibm-provider`` is now deprecated and will be removed
  in the next release. Users should migrate to :external+qiskit_ibm_runtime:doc:`qiskit-ibm-runtime <index>` following the
  `runtime migration guide
  <https://quantum.cloud.ibm.com/docs/migration-guides/qiskit-runtime-from-ibm-provider>`_.
  :external+qiskit_ibm_runtime:doc:`qiskit-ibm-runtime <index>` is not listed as a dependency for compatibility reasons, but users
  will need it to run experiments on IBM backends.


.. _Release Notes_0.6.0_Experiment Library Upgrades:

Experiment Library Upgrades
^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. releasenotes/notes/0.6/adjust-symbolic-pulses-amp-angle-representation-f5c40007416cf938.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- :class:`.HalfAngleCal` was changed from updating the complex amplitude of
  the pulse, to updating the angle in the (``amp``, ``angle``) representation. When used with
  the :class:`.FixedFrequencyTransmon` library, it will continue to work seamlessly
  in the new representation. However, when the experiment is used with custom
  built pulses, which rely on the old convention of complex ``amp`` (with no
  angle parameter) - the experiment will fail. Most reasonable cases will raise
  a detailed ``CalibrationError`` explaining the change and the way to adjust
  to it. Some edge cases - like a custom built pulse with an ``angle`` parameter
  which doesn't conform to the naming convention of Qiskit's
  ``ScalableSymbolicPulse`` class, or using a loaded calibration with ``complex``
  ``amp`` - will result in updating the wrong parameter.


.. _Release Notes_0.6.0_Experiment Class Upgrades:

Experiment Class Upgrades
^^^^^^^^^^^^^^^^^^^^^^^^^

.. releasenotes/notes/0.6/upgrade-remove-circuit-metadata-ec7d3c6b08781184.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- Removed unnecessary circuit metadata from the builtin experiment classes.
  Circuit metadata such as the associated qubit indices and experiment type
  are separately stored in the experiment metadata, and never used in the analysis.
  Removal of unnecessary circuit metadata compresses the job payload and
  thus is expected to benefit scalability.


.. _Release Notes_0.6.0_Curve Fit Upgrades:

Curve Fit Upgrades
^^^^^^^^^^^^^^^^^^

.. releasenotes/notes/0.6/add-dataframe-curve-data-a8905c450748b281.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- The behavior of :class:`.CurveAnalysis` data processing was changed.
  It used to raise ``DataProcessorError`` error when it encounters an experiment result
  which cannot be classified into any fit model, but this restriction was relaxed
  and the analysis continues with unclassified data.
  Unclassified data is just stored as-is in the :class:`.ScatterTable` with
  the null class ID assigned. Such data is ignored in the rest of analysis steps
  such as formatting, fitting, and visualization.


.. _Release Notes_0.6.0_Calibration Upgrades:

Calibration Upgrades
^^^^^^^^^^^^^^^^^^^^

.. releasenotes/notes/0.6/adjust-symbolic-pulses-amp-angle-representation-f5c40007416cf938.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- The representation of pulses in the :class:`.FixedFrequencyTransmon` library
  was changed from complex amplitude to (``amp``, ``angle``) representation. All pulses
  now include an ``angle`` parameter, and the default values of ``amp`` are set
  as type ``float`` instead of ``complex``.

.. releasenotes/notes/0.6/params_without_schedule-20555d98875a626b.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- The variables ``__drive_freq_parameter__`` and ``__readout_freq_parameter__``
  have been removed from :class:`.Calibrations`. These variables were given special
  treatment which is inconsistent with the framework. To replace them a
  mechanism to define and add parameters without a schedule has been added to
  the basis gate library. This has the added benefit of making the API of
  frequency calibration experiments more consistent with the other calibration
  experiments. Calibration developers can now add parameters to their library that are not
  attached to a schedule in a meaningful way.


.. _Release Notes_0.6.0_Deprecation Notes:

Deprecation Notes
-----------------

.. releasenotes/notes/0.6/deprecate-flatten-result-false-026a89c09cc7a004.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- Executing composite experiment and composite analysis with ``flatten_results=False``
  by default was deprecated. To create child experiment data, please explicitly
  set ``flatten_results=False``. The default value of ``flatten_results`` will be
  changed to ``True`` in the next release.

.. releasenotes/notes/0.6/deprecate-is-simulator-c101197a126e456f.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- :attr:`.BackendData.is_simulator` has been deprecated.
  :class:`~qiskit.providers.BackendV2` does not provide a standard interface
  for determining if a backend uses a simulator. Calling code must determine
  if a backend uses a simulator through some other means. Qiskit Experiments
  does not treat simulator-backed backends differently from hardware backed
  ones.

.. releasenotes/notes/0.6/experiment-artifacts-c481f4e07226ce9e.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- Setting the option ``return_data_points`` to ``True`` in curve analysis has been deprecated.
  Data points are now automatically provided in :class:`.ExperimentData` objects via the ``curve_data``
  artifact.

.. releasenotes/notes/0.6/experiment-artifacts-c481f4e07226ce9e.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- Direct access to the curve fit summary in :class:`.ExperimentData` has moved from
  :meth:`.analysis_results` to :meth:`.artifacts`, where values are stored in the
  :attr:`~.ArtifactData.data` attribute of :class:`.ArtifactData` objects. For example, to access the
  chi-squared of the fit, ``expdata.analysis_results(0).chisq`` is deprecated in favor of
  ``expdata.artifacts("fit_summary").data.chisq``. In a future release, the curve fit summary
  will be removed from :meth:`.analysis_results` and the option ``return_fit_parameters`` will be
  removed. For more information on artifacts, see the :doc:`artifacts how-to </howtos/artifacts>`.

.. releasenotes/notes/0.6/experiment-artifacts-c481f4e07226ce9e.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- Using numerical indices with :meth:`.ExperimentData.analysis_results`, including both integers and
  slices, is now deprecated. Access analysis results by analysis result name or ID instead.

.. releasenotes/notes/0.6/feature-support-calibrations-roundtrip-47f09bd9ff803479.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- Saving :class:`.Calibrations` instance into CSV file was deprecated.
  This only provides serialization for limited set of calibration data,
  and loading from the local file is not supported.

.. releasenotes/notes/0.6/feature-support-calibrations-roundtrip-47f09bd9ff803479.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- :meth:`.Calibrations.schedule_information` was deprecated.
  This method returns attached calibration templates in the string format,
  but this cannot be converted back to the original Qiskit representation.
  Now better serialization is provided with :meth:`.Calibrations.save` with JSON mode
  and it internally dumps these schedule in through QPY format.

.. releasenotes/notes/0.6/feature-support-calibrations-roundtrip-47f09bd9ff803479.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- :meth:`.Calibrations.load_parameter_values` was deprecated.
  Since saving :class:`.Calibrations` instance into the CSV format was deprecated,
  the required data file to invoke this method will be no longer generated
  in future calibrations instance. Full calibration instance roundtrip
  is now supported with the save and load method.

.. releasenotes/notes/0.6/feature-support-calibrations-roundtrip-47f09bd9ff803479.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- :meth:`.Calibrations.config` and :meth:`.Calibrations.from_config` were deprecated.
  Now canonical data representation is generated for calibration by the
  newly introduced :mod:`~qiskit_experiments.calibration_management.save_utils` module,
  and the legacy configuration dictionary is no longer used for JSON encoding.


.. _Release Notes_0.6.0_Bug Fixes:

Bug Fixes
---------

.. releasenotes/notes/0.6/0.6_release-4d766733941ad57a.yaml @ b'3b039c5df784748597261d38599c1c7cb2074377'

- Bug fixes are listed below in subsections by functional area.


.. _Release Notes_0.6.0_Experiment Library Fixes:

Experiment Library Fixes
^^^^^^^^^^^^^^^^^^^^^^^^

.. releasenotes/notes/0.6/fix-guess-rb-decay-f78e40a7d6d8dd67.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- Fixed a bug in :func:`~.rb_decay` where it unintentionally raises
  an ``IndexError`` if all ``y`` values are below ``b`` value
  so that it returns ``0`` for the case.

.. releasenotes/notes/0.6/half-angle-x-600debac368ce2c6.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- The :class:`.HalfAngle` experiment's circuits were changed so that they use
  combinations of ``rz`` and ``x`` instead of the less standard ``y`` gate.
  This change allows :class:`.HalfAngle` to be run on IBM backends directly.
  Previously, it could only be run through the :class:`.HalfAngleCal`
  subclass in combination with a :class:`.Calibrations` instance containing a
  custom calibration for the ``y`` gate.
  Fixes issue `#1233 <https://github.com/Qiskit-Community/qiskit-experiments/issues/1233>`_.

.. releasenotes/notes/0.6/plugable-rb-clifford-synthesis-0e66c62fa3088fba.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- Fixed a bug in circuit generation for three or more qubit Randomized Benchmarking where
  sampled Cliffords may be changed during their circuits synthesis
  (in the worst case, the resulting circuits may use qubits not in
  ``physical_qubits``). See issue
  `#1279 <https://github.com/Qiskit-Community/qiskit-experiments/issues/1279>`_
  for additional details.

.. releasenotes/notes/0.6/rabi-and-qv-bugfix-34636baee6651af1.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- Resolved a serialization issue that affected Rabi experiments when running it through
  the backend provider using custom amplitudes provided as a numpy array.

.. releasenotes/notes/0.6/rabi-and-qv-bugfix-34636baee6651af1.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- Resolved an issue that caused QV experiments to fail when executed via the backend provider using
  Qiskit for calculating ideal probabilities instead of Aer.

.. releasenotes/notes/0.6/rabi-and-qv-bugfix-34636baee6651af1.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- Resolved a serialization issue that affected DRAG experiments with integral beta values specified.


.. _Release Notes_0.6.0_Experiment Data Fixes:

Experiment Data Fixes
^^^^^^^^^^^^^^^^^^^^^

.. releasenotes/notes/0.6/bugfix_expdata_copy-2c73a21ad720858d.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- The :meth:`.ExperimentData.copy` method now copies the provider.

.. releasenotes/notes/0.6/exp-data-pickle-61511b6e926e3198.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- Fixed :mod:`pickle` deserialization of :class:`.ExperimentData` objects.
  Previously, :class:`.ExperimentData` objects could be serialized and
  deserialized using Python's ``pickle`` module, but deserialized objects
  were not completely restored and an exception would be raised when doing
  some operations like running analysis on the restored object. See `#1326
  <https://github.com/Qiskit-Community/qiskit-experiments/pull/1326/files>`__.

.. releasenotes/notes/0.6/experiment_data_fixes-f69c3569a8ba1342.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- Fixed a bug in :meth:`.ExperimentData._add_job_data` that caused job id
  related test fails.

.. releasenotes/notes/0.6/experiment_data_metadata_save_fix-912b7180a28cfb03.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- Fixed a bug in :class:`.ExperimentData` which caused experiment saves to the cloud service to fail when the metadata is large.

.. releasenotes/notes/0.6/experiment_data_save_bugfixes-48fe62bf2bfe38b8.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- The :attr:`.ExperimentData.auto_save` setter no longer saves sub-experiments twice.

.. releasenotes/notes/0.6/experiment_data_save_bugfixes-48fe62bf2bfe38b8.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- :meth:`.ExperimentData.save` now handles correctly figures in sub-experiments when ``flatten_results=True``.


.. _Release Notes_0.6.0_Visualization Fixes:

Visualization Fixes
^^^^^^^^^^^^^^^^^^^

.. releasenotes/notes/0.6/figure_return_SVG-4ad72fc8a3bee3cb.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- Figures loaded from the experiment service are now rendered correctly in Jupyter Notebook.


.. _Release Notes_0.6.0_API Changes for Experiment Authors:

API Changes for Experiment Authors
----------------------------------

.. releasenotes/notes/0.6/add-dataframe-curve-data-a8905c450748b281.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- Added the :meth:`~.CurveAnalysis._create_figures` method to the :class:`.CurveAnalysis` base class.
  A curve analysis subclass can overwrite this method to customize the output figures.
  The method is called with the :class:`.ScatterTable` containing all intermediate data points
  generated during the curve analysis.

.. releasenotes/notes/0.6/add-test-equality-checker-dbe5762d2b6a967f.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- Added the :meth:`QiskitExperimentsTestCase.assertEqualExtended` method for generic equality checks
  of Qiskit Experiments class instances in unittests. This is a drop-in replacement of
  calling the assertTrue with :meth:`QiskitExperimentsTestCase.json_equiv`.
  Note that some Qiskit Experiments classes may not officially implement equality check logic,
  although objects may be compared during unittests. Extended equality check is used
  for such situations.

.. releasenotes/notes/0.6/add-test-equality-checker-dbe5762d2b6a967f.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- The following unittest test case methods will be deprecated:

    * :meth:`QiskitExperimentsTestCase.json_equiv`
    * :meth:`QiskitExperimentsTestCase.ufloat_equiv`
    * :meth:`QiskitExperimentsTestCase.analysis_result_equiv`
    * :meth:`QiskitExperimentsTestCase.curve_fit_data_equiv`
    * :meth:`QiskitExperimentsTestCase.experiment_data_equiv`

  One can now use the :func:`~test.extended_equality.is_equivalent` function instead.
  This function internally dispatches the logic for equality check.

.. releasenotes/notes/0.6/add-test-equality-checker-dbe5762d2b6a967f.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- The default behavior of :meth:`QiskitExperimentsTestCase.assertRoundTripSerializable` and
  :meth:`QiskitExperimentsTestCase.assertRoundTripPickle` when ``check_func`` is not
  provided was upgraded. These methods now compare the decoded instance with
  :func:`~test.extended_equality.is_equivalent`, rather than
  delegating to the native ``assertEqual`` unittest method.
  One writing a unittest for serialization no longer need to explicitly set checker function.

.. releasenotes/notes/0.6/device-component-c9ec9011c529425c.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- A ``device_component`` field that contains a list of device components used in the experiment
  has been added to experiment metadata. Experiments with non-qubit components should override the
  default value of all qubit components. See the :doc:`custom experiments tutorial
  </tutorials/custom_experiment>` for more details.


.. _Release Notes_0.6.0_Other Notes:

Other Notes
-----------

.. releasenotes/notes/0.6/add_warning_analysis_without_data-bfc802da52591f13.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- Display a warning when running an analysis on :class:`.ExperimentData` objects which do not contain data.

.. releasenotes/notes/0.6/adjust-symbolic-pulses-amp-angle-representation-f5c40007416cf938.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- Qiskit 0.23.0 began phasing out support of complex parameters
  in the Pulse module. Mainly, all library symbolic pulses were converted
  from complex amplitude representation to a duo of real (float) parameters
  (``amp``, ``angle``). To avoid problems, Qiskit Experiments adopted this convention.

  Changes were made to :class:`.FixedFrequencyTransmon` and :class:`.HalfAngleCal`
  (see upgrade section). With the exception of :class:`.HalfAngleCal`, all
  library experiments should continue to function as they did before (even with
  complex ``amp``). When used with the :class:`.FixedFrequencyTransmon` library,
  :class:`.HalfAngleCal` will also continue working as before.

  Eventually, support for complex parameters will be dropped altogether, and it is
  thus pending deprecation - including for saving and loading calibration data with
  complex values.

.. releasenotes/notes/0.6/requirements-extras-d5768794acbce467.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- A new set of optional dependencies have been split off from the developer dependencies and
  can be installed separately as ``qiskit-experiments[extras]``. These are packages that enable
  optional experiment features such as ``scikit-learn``-based discriminators. Qiskit Dynamics and
  Qiskit Aer have also been marked as optional in this manner.

.. releasenotes/notes/0.6/update-figure-name-2db258c30ffe9912.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- Figure names have been updated to include qubit indices up to the first five device components in
  the experiment, with format ``StandardRB_Q0_Q1_Q2_Q3_Q5_b4f1d8ad.svg``. For composite
  experiments where ``flatten_results`` is set to ``True``, the head of the figure name is now the
  class name of the experiment instead of ``ParallelExperiment`` or ``BatchExperiment``, such that
  the figure name is the same when ``flatten_results`` is ``False``. The behavior when a figure
  name is repeated and ``overwrite`` is ``False`` has changed from throwing an exception to
  appending a numerical suffix to the figure name like ``StandardRB_Q0_Q1_Q2_Q3_Q5_b4f1d8ad-1.svg``.

.. releasenotes/notes/0.6/update-figure-name-2db258c30ffe9912.yaml @ b'e8531c4f6af9432827bc28c772c5a179737f0c3c'

- Figure metadata now includes ``experiment_type`` and ``device_components``.


.. _Release Notes_0.5.4:

0.5.4
=====

.. _Release Notes_0.5.4_Prelude:

Prelude
-------

.. releasenotes/notes/0.5/0_5_4_release-ed63a0651f74db28.yaml @ b'cb8341016e5100787611a10277866ddcab8d6fac'

Qiskit Experiments 0.5.4 is a minor improvement and fixes release.

.. _Release Notes_0.5.4_Bug Fixes:

Bug Fixes
---------

.. releasenotes/notes/fix-curve-fit-weights-fb43d3aa5ed1c91c.yaml @ b'8bf58e97a005fbdf10cf1eee8f455bd23d746177'

- Fixed calculation of weight for curve fitting. Previously the weights of data points to obtain
  the residual of fit curve were computed by the inverse of the error bars of y data.
  This may yield significant weights on certain data points when their error bar is small or zero,
  and this can cause the local overfit to these data points.
  To avoid this edge case of small error bars, computed weights are now clipped at 90 percentile.
  This update might slightly change the outcome of fit.

.. releasenotes/notes/rb-v2-none-coupling-fda2b22afdef507b.yaml @ b'ab07b8e3ddb8844a9a481c2c98688d9291d2edb6'

- Changed :class:`.StandardRB` to treat two qubit operations in the
  :class:`qiskit.transpiler.Target` as having all-to-all connectivity if
  there is no set of specific pairs of coupled qubits. Most importantly, this
  change allows :class:`.StandardRB` to work with
  :class:`qiskit_aer.AerSimulator` for multi-qubit benchmarking after
  ``qiskit-aer`` 0.13.0. Version 0.13.0 of ``qiskit-aer`` changed
  the default :class:`qiskit_aer.AerSimulator` to have such a
  :class:`qiskit.transpiler.Target` without specific coupled pairs.
  See `#1292 <https://github.com/Qiskit-Community/qiskit-experiments/issues/1292>`__.


.. _Release Notes_0.5.4_Other Notes:

Other Notes
-----------

.. releasenotes/notes/remove-tomo-reset-3f21ec4d0dacba91.yaml @ b'cb8341016e5100787611a10277866ddcab8d6fac'

- Removed the reset instruction from the beginning of tomography experiments.
  Since qubits are usually reset between circuits, this change should have no
  impact on tomography experiments, but it should allow backends that do not
  provide a reset instruction to run tomography experiments. See `#1250
  <https://github.com/Qiskit-Community/qiskit-experiments/issues/881>`__.


.. _Release Notes_0.5.3:

0.5.3
=====

.. _Release Notes_0.5.3_Prelude:

Prelude
-------

.. releasenotes/notes/0.5/0_5_3_release-71ba547279508401.yaml @ b'a77a57a24195c6b1a9a81c083c0e607cfa4d3e76'

Qiskit Experiments 0.5.3 is a minor improvement and fixes release.


.. _Release Notes_0.5.3_New Features:

New Features
------------

.. releasenotes/notes/irb-circuit-order-619845a707519c44.yaml @ b'920c4a5793d97de6d5bb1c61a0884bb69fe07723'

- A new experiment option ``circuit_order`` was added to :class:`~.InterleavedRB`.
  It allows to change the order of the reference and the interleaved circuits
  and hence slightly alter the impact of noise on interleaved RB results.
  The default value is set to ``"RIRIRI"`` that alternate a reference and
  an interleaved circuit.


.. _Release Notes_0.5.3_Bug Fixes:

Bug Fixes
---------

.. releasenotes/notes/irb-circuit-order-619845a707519c44.yaml @ b'920c4a5793d97de6d5bb1c61a0884bb69fe07723'

- Changed the ordering of circuits generated by :class:`~.InterleavedRB` back to
  RIRIRI (R: Reference, I: Interleaved) order.
  It was accidentally changed into RRRIII order in
  `#898 <https://github.com/Qiskit/qiskit-experiments/pull/898>`_.
  Before that, it had been RIRIRI order.


.. _Release Notes_0.5.2:

0.5.2
=====

.. _Release Notes_0.5.2_Prelude:

Prelude
-------

.. releasenotes/notes/0.5/0_5_2_release-3be0f1395ff73aed.yaml @ b'cc74e355d1e76f8903876c9a02baa190385cc685'

Qiskit Experiments 0.5.2 is a minor bug fix and performance improvement release.


.. _Release Notes_0.5.2_New Features:

New Features
------------

.. releasenotes/notes/attach-other-cals-2f539e7799ceb6c8.yaml @ b'cc74e355d1e76f8903876c9a02baa190385cc685'

- A new method :meth:`.qiskit_experiments.calibration_management.Calibrations.has_template`
  has been added to :class:`~.qiskit_experiments.calibration_management.Calibrations`
  to check if a template schedule exists for a particular set of qubits.


.. _Release Notes_0.5.2_Bug Fixes:

Bug Fixes
---------

.. releasenotes/notes/attach-other-cals-2f539e7799ceb6c8.yaml @ b'cc74e355d1e76f8903876c9a02baa190385cc685'

- :class:`.FineXDragCal` and :class:`.EFRoughXSXAmplitudeCal` were updated to
  attach ``sx`` and ``x`` calibrations to their circuits, respectively.
  Previously, they only attached the ``x`` and ``x12`` calibrations that they
  were calibrating. See issue `#1158 <https://github.com/Qiskit/qiskit-experiments/issues/1158>`_.


.. _Release Notes_0.5.2_Other Notes:

Other Notes
-----------

.. releasenotes/notes/0.5/0_5_2_release-3be0f1395ff73aed.yaml @ b'cc74e355d1e76f8903876c9a02baa190385cc685'

- The performance of experiment analysis for parallel experiments has been improved
  significantly due to improved results marginalization. See PR
  `#1144 <https://github.com/Qiskit/qiskit-experiments/pull/1144>`_ for more details.


.. _Release Notes_0.5.1:

0.5.1
=====

.. _Release Notes_0.5.1_Prelude:

Prelude
-------

.. releasenotes/notes/0.5/0_5_1_release-e445b6cc64742cc0.yaml @ b'99105a682d0f14bb9b6046430d83a30b86168c25'

Qiskit Experiments 0.5.1 is a minor bug fix release.

.. _Release Notes_0.5.1_Bug Fixes:

Bug Fixes
---------

.. releasenotes/notes/backend-in-rough-frequency-cal-8582610249e2327b.yaml @ b'2a089243eb94dc11061e3682e6e6bb6c44d09fbe'

- Added a missing ``backend`` parameter to :class:`~.RoughEFFrequencyCal` and
  exposed it in the experiment library.

.. releasenotes/notes/cals-no-coupling-map-5114ae9faa2f9e69.yaml @ b'ccbd5123700573ed5cfb7abf187834bd0601206c'

- Fixed error generating circuits for :class:`.BaseCalibrationExperiment`
  subclasses when the backend instance had no coupling map. Fixed `#1116
  <https://github.com/Qiskit/qiskit-experiments/issues/1116>`_.

.. releasenotes/notes/matplotlib-fix-58d938b49771cf17.yaml @ b'38f26aa40e31cf2f30f73b7ae44fc62bac096c49'

- Fixed a deprecated Matplotlib ``MarkerStyle`` usage in the visualization module that was causing warnings in Matplotlib 3.6+.


.. _Release Notes_0.5.0:

0.5.0
=====

.. _Release Notes_0.5.0_Prelude:

Prelude
-------

.. releasenotes/notes/0.5/0_5_release-89f59845afb19e89.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

The Qiskit Experiments 0.5 release brings various improvements and bug fixes. Notable changes include the visualization module for drawing figures, which replaces the previous plotting functionality. The speed of randomized benchmarking experiments has been significantly improved. The ``qubit`` and ``qubits`` input to experiments has been regularized to ``physical_qubits``, and support for ``qiskit-ibmq-provider`` has been deprecated in favor of ``qiskit-ibm-provider``. New experiments added include :class:`.MultiStateDiscrimination`, :class:`.ZZRamsey`, :class:`.MitigatedStateTomography`, and :class:`.MitigatedProcessTomography`, along with significant improvements to other tomography experiments. The documentation has been significantly refactored and introductory tutorials have been added.

.. _Release Notes_0.5.0_New Features:

New Features
------------

.. releasenotes/notes/0.5/T1_experiment_level_1_mesurment_analysis-078db79e3b0c16b9.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Added new class :class:`.T1KerneledAnalysis`. This class is used for the :class:`.T1`
  experiment with the option ``meas_level=MeasLevel.KERNELED``. The analysis
  normalizes the data and fixes its orientation.

.. releasenotes/notes/0.5/add-new-visualization-module-9c6a84f2813459a7.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Added a new visualization module to plot figures and draw onto figure canvases. The new module contains
  plotters and drawers, which integrate with :class:`.CurveAnalysis` but can be used independently of the
  analysis classes. This module replaces the old and now deprecated
  ``qiskit_experiments.curve_analysis.visualization`` submodule.

.. releasenotes/notes/0.5/add-new-visualization-module-9c6a84f2813459a7.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Added a new IQ plotting class :class:`.IQPlotter` for plotting IQ/level-1 data (individual
  shots and their average) and a discriminator that classifies the data into
  states.

.. releasenotes/notes/0.5/add-new-visualization-module-9c6a84f2813459a7.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Added a new ``image()`` method to :class:`.BaseDrawer` and :class:`.MplDrawer` to plot
  two-dimensional images on a figure canvas.

.. releasenotes/notes/0.5/backend-timing-bc05fd3cc7b41a45.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Introduced a new class :class:`qiskit_experiments.framework.BackendTiming`, which
  provides helper methods for rounding pulse and delay times to values
  compatible with a backend's timing constraints.

.. releasenotes/notes/0.5/curve-analysis-4bcc10cf3a39a85d.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- New :class:`.BaseCurveAnalysis` option ``average_method`` has been added. This option
  modifies an averaging technique for y values over the same x values.
  It defaults to ``sample`` for the RB experiments and ``shots_weighted`` for the rest of analysis.
  Previously the setup was hardcoded in the ``_format_data`` method of the analysis class,
  and no statistical difference has been introduced with introduction of this option.

.. releasenotes/notes/0.5/ecr_lib-381cb18885e81abd.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- A new basis gate library called :class:`.EchoedCrossResonance` has been added.

.. releasenotes/notes/0.5/initial_circuit_resonator_spectroscopy-38914d54655033da.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Added a new ``initial_circuit`` option to :class:`.ResonatorSpectroscopy` for appending
  before measurements. This can be used to run resonator spectroscopy with different qubit states.

.. releasenotes/notes/0.5/multi-state-discrimination-experiment-59344a21f9e99ca3.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Added a new experiment :class:`.MultiStateDiscrimination` for classifying IQ
  clusters of multi-level energy states.

.. releasenotes/notes/0.5/multi-state-discrimination-experiment-59344a21f9e99ca3.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Added a new sklearn discriminator class :class:`~qiskit_experiments.data_processing.SkQDA`.

.. releasenotes/notes/0.5/pulse-backend-for-test-tutorials-fad8b77615ff09e5.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Added pulse simulator backends :class:`.PulseBackend` and subclass :class:`.SingleTransmonTestBackend`
  that use Qiskit Dynamics to simulate pulse schedules included in
  the calibrations attached to transpiled quantum circuits. The backend is capable of
  simulating level one (IQ) and level two (counts) data. The main purpose of this
  backend is to make the test suite more realistic and allow for tutorials that
  do not require hardware backends.

.. releasenotes/notes/0.5/py311-49f08e1e0350c6b7.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- qiskit-experiments has been marked as compatible with Python 3.11 in the
  package metadata. qiskit-experiments currently tests against Python 3.7,
  3.8, 3.9, 3.10, and 3.11.

.. releasenotes/notes/0.5/ramsey_xy-4123317b014db3b0.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- The algorithm to estimate fit parameter guesses in :class:`.RamseyXYAnalysis`
  has been upgraded.
  The previous algorithm was not robust to experiment outcomes with low frequency,
  where Ramsey X and Y curves almost remain at P=1.0 and 0.5, respectively.
  The new algorithm also offers reliable initial guesses for such situations.
  In addition, the number of frequency guesses has been increased to cover the uncertainty of FFT.

.. releasenotes/notes/0.5/readout-error-c95b99ae5a6ba7ac.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Added a ``backend`` init kwarg to the :class:`.LocalReadoutError` and
  :class:`.CorrelatedReadoutError` experiments, and the
  ``physical_qubits`` kwarg has been made optional. If a backend is supplied without
  specifying physical qubits, the experiment will be initialized on all
  qubits for the backend.

.. releasenotes/notes/0.5/separate-jobs-686711fba530820d.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Added a new experiment option for batch experiments called ``separate_jobs``. If set
  to ``True``, then circuits of different sub-experiments will be routed to different
  jobs. Default value is ``False``.

.. releasenotes/notes/0.5/separate-jobs-686711fba530820d.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Added a ``max_circuits`` experiment option to :class:`~.BaseExperiment` to allow
  specifying the max number of circuits per job when running an experiment.
  If set to ``None`` (default), the max circuits per job is determined by the
  backend. If both the option value and backend value are not ``None``, the
  miniminum of the two values will be used for job splitting.

.. releasenotes/notes/0.5/tomography-b091ce13d6983bc1.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Added ``backend``, ``analysis``, and ``target`` init kwargs to the
  :class:`~.StateTomography` and :class:`~.ProcessTomography` experiments.
  These allow specifying the intended backend, a custom analysis class, or a
  custom target for fidelity calculations when initializing the experiments.

.. releasenotes/notes/0.5/tomography-b091ce13d6983bc1.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Improved :class:`.LocalMeasurementBasis` and :class:`.LocalPreparationBasis`
  tomography basis classes support for initializing a noisy basis for
  performing state preparation and measurement error mitigated
  :class:`.StateTomography` and :class:`.ProcessTomography` experiments.

  For preparation bases, a noisy reset operation on a specific qubit,
  or subset of qubits, can now be input as a quantum channel, and the
  noisy prepared states are generated by applying the ideal instructions
  to the noisy initial state.

  For measurement bases, a noisy POVM or quantum channel can be supplied for
  the 0-index basis (typically the Z-basis), and other bases index POVMs will
  be generated by applying the ideal inverse instructions to the noisy POVMS.

.. releasenotes/notes/0.5/tomography-b091ce13d6983bc1.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Added an optional ``mitigator`` kwarg to :class:`.PauliMeasurementBasis`
  which can be used to initialize the basis with a
  :class:`~qiskit.result.LocalReadoutMitigator` to construct a readout error mitigated
  basis for use with :class:`.StateTomography` and
  :class:`.ProcessTomography` experiments.

  The :class:`.LocalReadoutError` experiment can be run to obtain the
  :class:`~qiskit.result.LocalReadoutMitigator` from its analysis results.

.. releasenotes/notes/0.5/tomography-b091ce13d6983bc1.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Added readout error mitigated tomography experiments
  :class:`.MitigatedStateTomography` and :class:`.MitigatedProcessTomography`.
  These are both implemented as a :class:`.BatchExperiment` consisting of a
  :class:`.LocalReadoutError` characterization experiment followed by either
  a :class:`.StateTomography` or :class:`.ProcessTomography` experiment.

.. releasenotes/notes/0.5/tomography-b091ce13d6983bc1.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Added support for conditional tomographic reconstruction to the
  :class:`.StateTomography` and :class:`.ProcessTomography` experiments.

  There are three types of conditioning that can be used independently
  or together in any combination for reconstruction of a state or
  channel as a list of components conditional on these values.

  The ``conditional_circuit_clbits`` init option can be used to
  specify any subset of clbits in an tomography circuit containing
  clbits to be conditioned on when peforming the tomographic
  reconstruction. The conditioning outcome value of the clbits is
  stored in the analysis results ``extra`` field.

  The ``conditional_measurement_indices`` analysis option can be used
  to condition on the measurement basis index and outcome value of a
  specific subset of tomographic basis measurements. The conditioning
  basis index and outcome value are both stored in the analysis
  results ``extra`` field.

  The ``conditional_preparation_indices`` analysis option can be used
  to condition on the preparation basis index of a specific subset of
  tomographic basis preprations. The conditioning basis index is stored
  in the analysis results ``extra`` field.

.. releasenotes/notes/0.5/tomography-b091ce13d6983bc1.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Adds an option to :class:`~.StateTomographyAnalysis` and
  :class:`~.ProcessTomographyAnalysis` to bootstrap error bars on state
  and process fidelity analysis results. This can be activated by setting
  the ``target_bootstrap_samples`` analysis option to a value.

  Note that bootstrapping involves re-running the full tomography fit on
  re-samples of tomography outcome data for each measurement basis and
  hence the total analysis time will increase linearly with the number
  of bootstrap samples.

.. releasenotes/notes/0.5/update-cr-hamtomo-with-duration-380da3452045cd0c.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- The :class:`.CrossResonanceHamiltonian` experiment and its subclass now accept
  ``durations`` with default values. Note that values should be provided in
  units of seconds rather than samples, and must include pulse ramps at edges.
  Default values with linear increment are generated according to new experiment options,
  ``min_duration``, ``max_duration``, and ``num_durations``, when the durations
  are not explicitly provided. The default values are chosen by assuming a
  ZX rate of around 1 MHz which is typical for IBM Quantum backends.
  User can update these option values as well as provide full ``durations``
  to tailor experiment settings to their device.
  Total durations should be carefully chosen not to overflow the waveform memory
  when the experiment is run on a real hardware. With this update,
  the minimum example code to run this experiment might be

  .. code-block:: python

    from qiskit_experiments.library.characterization import CrossResonanceHamiltonian

    expr = CrossResonanceHamiltonian(qubits=(0, 1), amp=0.3, backend=backend)
    exp_data = expr.run()

  where the durations to scan are implicitly set by experiment options.

.. releasenotes/notes/0.5/zz-220e3c0894dd9076.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- A new experiment :class:`.ZZRamsey` has been added to measure the ZZ
  coefficient between a pair of qubits.


.. _Release Notes_0.5.0_Known Issues:

Known Issues
------------

.. releasenotes/notes/0.5/pulse-backend-for-test-tutorials-fad8b77615ff09e5.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- The :class:`.PulseBackend` only supports single qubit operations and will be upgraded in the future.


.. _Release Notes_0.5.0_Upgrade Notes:

Upgrade Notes
-------------

.. releasenotes/notes/0.5/0_4_deprecations-6e5efbaeeb870184.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Several deprecated methods and options have been removed and will no longer work:

  * ``BaseExperiment.analysis_options`` has been removed. ``experiment.analysis.options``
    should be used instead.
  * The ``__analysis_class__`` attribute of :class:`.BaseAnalysis` has been removed.
    Use the ``analysis`` kwarg of ``BaseExperiment.__init__`` to specify a default
    analysis class.
  * The ``component_experiment_data()`` method has been removed from
    :class:`.ExperimentData` and replaced by :meth:`.ExperimentData.child_data`.
  * The ``CompositeExperiment.component_analysis`` method has been removed. Component
    analysis classes should be directly accessed using
    :meth:`.CompositeAnalysis.component_analysis`.
  * The ``library`` argument to :class:`.Calibrations` has been removed and replaced by
    ``libraries``.
  * The class attribute ``CurveAnalysis.__fixed_parameters__`` has been removed.
    The ``fixed_parameters`` analysis option should be set instead.
  * The method ``CurveAnalysis._data()`` has been removed.
  * The :class:`.CurveAnalysis` attribute ``__series__`` has been removed and is
    replaced by the constructor argument.
  * The ``FineDragAnalysis``, ``FineFrequencyAnalysis``, and ``FineHalfAngleAnalysis``
    analysis classes have been removed and replaced by
    :class:`.ErrorAmplificationAnalysis`.
  * Randomized benchmarking utility functions ``get_error_dict_from_backend()``,
    ``count_ops()``, ``gates_per_clifford()``, ``calculate_1q_epg()``, and
    ``calculate_2q_epg()`` have been removed from :class:`.RBUtils` and replaced by
    methods in the RB experiment and analysis themselves.
  * The ``error_dict`` analysis option of :class:`.RBAnalysis` has been removed and
    merged into the analysis option ``gate_error_ratio``.

.. releasenotes/notes/0.5/cal_transpiling-467fa52cde966fbf.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Transpilation in the calibration experiments has been upgraded. Calibration
  experiments define a carefully chosen set of gates and pulses
  that the transpiler should not modify. If these gates are modified by
  transpilation the results may be unusable. :class:`.BaseCalibrationExperiment`
  now defines its own transpilation to ensure a proper execution of the
  experiments. Transpile options are no longer needed for calibration
  experiments.

.. releasenotes/notes/0.5/change-rb-transpile-option-188fd196c0c0d983.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- The default transpile option value of ``optimization_level`` for RB experiments
  (:class:`~.StandardRB` and :class:`~.InterleavedRB`) was changed from ``0`` to ``1``
  in order to reduce the number of gates in transpiled circuit and hence
  circuit generation/excution time and circuit sample variance in P(0) value.
  This is not an API change but, after this change, you will observe slower decay curves
  than before if you use the default configuration. And if you want to reproduce the results
  you obtained before this change, you may need to set ``optimization_level=0`` with
  :meth:`set_transpile_options`.

.. releasenotes/notes/0.5/removed-ibmq-provider-1c757ce5ef01fb19.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- ``qiskit-ibmq-provider`` is deprecated and has been dropped as a requirement. Interactions
  with IBM backends should use the ``qiskit-ibm-provider`` package instead (must be installed
  separately; see the
  `migration guide <https://qiskit.org/documentation/partners/qiskit_ibm_provider/tutorials/Migration_Guide_from_qiskit-ibmq-provider.html>`_
  for more details).

.. releasenotes/notes/0.5/t2backend-554b3edd4862d334.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- The :class:`qiskit_experiments.test.t2hahn_backend.T2HahnBackend` was
  refactored to use the simulator from qiskit-aer which provides better
  performance. As part of the refactoring, support was removed for passing
  qubit parameters (e.g.  ``t2hahn``, ``frequency``,
  ``initialization_error``, etc.) as single element lists when the backend
  has more than one qubit. These arguments need to be passed as numbers that
  apply to all qubits or sequences of numbers with one entry for each qubit.
  If passing numbers for a backend to represent more than one qubit, at least
  one parameter must be passed as a sequence or the ``num_qubits`` parameter
  must be passed to indicate how many qubits the backend should simulate.
  Additionally, passing ``None`` for these arguments was deprecated. The
  value that makes that option have no effect should be used instead (for
  example, ``0.0`` for ``initialization_error``).

.. releasenotes/notes/0.5/tomography-b091ce13d6983bc1.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Renamed the ``qubits``, ``measurement_qubits``, and ``preparation_qubits``
  init kwargs of :class:`~.StateTomography`,
  :class:`~.ProcessTomography`, and :class:`.TomographyExperiment` to
  ``physical_qubits``, ``measurement_indices`` and ``preparation_indices``
  respectively. This is to make the intended use of these kwargs more clear
  as the measurement and preparation args refer to the index of circuit
  qubits in the physical qubits list, not the physical qubit values
  themselves.

.. releasenotes/notes/0.5/tomography-b091ce13d6983bc1.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- The automatic overriding of the default CVXPY SDP solver for the
  :func:`.cvxpy_gaussian_lstsq` and :func:`.cvxpy_linear_lstsq` has been disabled
  and will now use the default SDP solver of CVXPY unless a custom solver
  is set using the ``fitter_options`` analysis options.

.. releasenotes/notes/0.5/tomography-b091ce13d6983bc1.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- The ``weights`` kwarg of the :func:`.cvxpy_linear_lstsq` and
  :func:`.scipy_linear_lstsq` tomography fitters has been changed to accept
  a weights array the same shape as the supplied ``outcome_data`` array.

.. releasenotes/notes/0.5/tphi-option-025f02c2c843c74f.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- The :class:`.Tphi` has been changed to use :class:`.T2Hahn` as the default T2
  estimate because it provides a more meaningful measurement on superconducting
  devices. An option ``t2type`` has been added to allow the user to toggle between
  using :math:`T_2^*` from :class:`.T2Ramsey` by specifying "ramsey" or :math:`T_2`
  from :class:`.T2Hahn`, which is the default value "hahn".


.. _Release Notes_0.5.0_Deprecation Notes:

Deprecation Notes
-----------------

.. releasenotes/notes/0.5/add-new-visualization-module-9c6a84f2813459a7.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Deprecated the :mod:`qiskit_experiments.curve_analysis.visualization` submodule and replaced it with the new
  :mod:`qiskit_experiments.visualization` submodule.

.. releasenotes/notes/0.5/curve-analysis-4bcc10cf3a39a85d.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Providing ``data_sort_key`` directly to the LMFIT model to instantiate :class:`.CurveAnalysis`
  has been deprecated. This option is not officially supported by the LMFIT,
  and thus curve analysis cannot guarantee this option is properly managed
  in all LMFIT model subclasses.

.. releasenotes/notes/0.5/pulse-backend-for-test-tutorials-fad8b77615ff09e5.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- :class:`~qiskit_experiments.test.mock_iq_helpers.MockIQRabiHelper` is now deprecated and
  should be replaced with :class:`.SingleTransmonTestBackend`.

.. releasenotes/notes/0.5/qubit-deprecate-13f123c35f0a3535.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Experiment constructor arguments ``qubit`` and ``qubits`` have been renamed
  ``physical_qubits``. For the ``qubit`` case, the argument type has changed
  from an integer to a sequence with a single integer. For example,
  ``FineXAmplitude(0)`` becomes ``FineXAmplitude([0])``.

.. releasenotes/notes/0.5/tomography-b091ce13d6983bc1.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- The ``qubits``, ``measurement_qubits``, and ``preparation_qubits``
  init kwargs of :class:`~.StateTomography`,
  :class:`~.ProcessTomography`, and :class:`~.TomographyExperiment` have
  been deprecated. They have been replaced with kwargs ``physical_qubits``,
  ``measurement_indices`` and ``preparation_indices`` respectively. The
  renamed kwargs have the same functionality as the deprecated kwargs.

.. releasenotes/notes/0.5/update-cr-hamtomo-with-duration-380da3452045cd0c.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- The ``flat_top_widths`` argument and experiment option of
  :class:`.CrossResonanceHamiltonian` experiment and its subclass
  have been deprecated and will be removed in Qiskit Experiments 0.6.

.. releasenotes/notes/0.5/update-number-to-2q-clifford-mapping-c28f1f29b0205d57.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Two helper methods :meth:`.CliffordUtils.random_cliffords` and
  :meth:`.CliffordUtils.random_clifford_circuits` have been deprecated. Their functionality
  are now incorporated into :meth:`.StandardRB.__sample_sequence`.


.. _Release Notes_0.5.0_Bug Fixes:

Bug Fixes
---------

.. releasenotes/notes/0.5/analysis-replace-results-bug-fix-2d1a77921f5ec22e.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Fixed a bug where old analysis results were saved in the case of a failed analysis. Now analysis
  results will be cleared before running :meth:`~.BaseAnalysis._run_analysis`. As a result, when analysis fails, an
  empty analysis result will be saved to the database service.

.. releasenotes/notes/0.5/calibration-backendv2-e564f466eb1c9999.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Updated querying of :class:`~qiskit.providers.Backend` metadata to support
  the look up of qubit T1 and measurement drive frequency, in order to
  support :class:`~qiskit.providers.BackendV2` backends. The look up of the
  latter is ``qiskit-ibm-provider`` specific. This change fixed errors
  failing to find these properties when using :class:`.ResonatorSpectroscopy`
  (issue `#1099 <https://github.com/Qiskit/qiskit-experiments/issues/1099>`_)
  and when using restless measurements with ``BackendV2`` backends.

.. releasenotes/notes/0.5/fix-drag-reanalysis-46f4c6679555242d.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Fixed a bug where redoing :meth:`.DragCalAnalysis.run` generated wrong fit models.

.. releasenotes/notes/0.5/fix-matplotlib-3.6.0-failing-test-5a747f61a9c357b4.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Fixed a bug where :class:`.CurveAnalysis` tests would fail with matplotlib 3.6.0 owing to a deprecated
  function call used in :class:`.MplCurveDrawer`. The new :class:`.MplCurveDrawer` no longer uses the
  deprecated function.

.. releasenotes/notes/0.5/fix-missing_calibration_updator_call-a255b28dd1449ea4.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Fixed a bug where :class:`.Calibrations` was not updated when calibration
  experiments were run through the composite experiment framework.

.. releasenotes/notes/0.5/readout-error-c95b99ae5a6ba7ac.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Fixed a bug in the :class:`~.LocalReadoutError` experiment where analysis
  would fail when run on an ideal simulator with no readout error. See
  `Issue #992 <https://github.com/Qiskit/qiskit-experiments/issues/992>`_
  for additional details.

.. releasenotes/notes/0.5/sklearn-imports-c82155c0a2c81811.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- The importing of ``scikit-learn`` was moved from module-level imports
  inside of ``try`` blocks to dynamic imports at first usage of the
  ``scikit-learn`` specific feature. This change should avoid errors in the
  installation of ``scikit-learn`` from preventing a user using features of
  ``qiskit-experiments`` that do not require ``scikit-learn``. See `#1050
  <https://github.com/Qiskit/qiskit-experiments/issues/1050>`_.

.. releasenotes/notes/0.5/target-none-properties-2190e45d5d69cc60.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Fixed :meth:`.BackendData.coupling_map` and
  :meth:`.BackendData.drive_freqs` raising exceptions when the underlying
  backend has ``None`` for its coupling and qubit property entries. Also,
  changed :meth:`.BackendData.control_channel` to return an empty list rather
  than ``None`` when there is no control channel data.
  See `#1035 <https://github.com/Qiskit/qiskit-experiments/issues/1035>`__.

.. releasenotes/notes/0.5/tomo-barriers-aae4aafedaca5c3d.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Fixed qpy serialization and deserialization of tomography experiments. The
  barrier instructions in tomography experiments were created with the wrong
  Python type which qpy did not support. This issue was most acute when using
  ``qiskit-ibm-provider`` which submits circuits to the provider using qpy.
  There could have been subtler issues with circuit timing using a different
  provider if the barriers were not separating important circuit
  instructions. See `#1060 <https://github.com/Qiskit/qiskit-experiments/issues/1060>`_.

.. releasenotes/notes/0.5/tomography-b091ce13d6983bc1.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Fixed bug in :class:`~.StateTomography` and :class:`~.ProcessTomography`
  experiments where if the input circuit contained conditional instructions
  with multiple classical registers the tomography measurement circuits
  would contain incorrect conditionals due to a bug in the
  :meth:`qiskit.circuit.QuantumCircuit.compose` method.

  See `Issue #942 <https://github.com/Qiskit/qiskit-experiments/issues/943>`_
  for additional details.

.. releasenotes/notes/0.5/uarray-warning-d4c38566a510e58f.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- A ``RuntimeWarning`` will no longer be generated by ``numpy`` when running a
  data processor on level one data. See `#1071
  <https://github.com/Qiskit/qiskit-experiments/issues/1071>`_.

.. releasenotes/notes/0.5/update-cr-hamtomo-with-duration-380da3452045cd0c.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Fixed a bug where the :class:`.EchoedCrossResonanceHamiltonian` experiment
  overestimated Hamiltonian coefficients by a factor of 2.


.. _Release Notes_0.5.0_API Changes for Experiment Authors:

API Changes for Experiment Authors
----------------------------------

.. releasenotes/notes/0.5/curve-analysis-4bcc10cf3a39a85d.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- To map experiment result data to a particular LMFIT model in :class:`.CurveAnalysis`,
  an author must provide the ``data_subfit_map`` analysis option rather than directly binding
  ``data_sort_key`` with the target LMFIT model.
  The ``data_subfit_map`` option is a dictionary keyed on the model name. For example,

  .. code-block:: python3

    class MyAnalysis(CurveAnalysis):

      def __init__(self):
        super().__init__(
          models=[
            lmfit.models.ExpressionModel(expr="x+a0", name="expr1"),
            lmfit.models.ExpressionModel(expr="x+a1", name="expr2"),
          ]
        )

      @classmethod
      def _default_options(cls) -> Options:
        options = super()._default_options()
        options.data_subfit_map = {"expr1": {"tag": "1"}, "expr2": {"tag": "2"}}
        return options

  As shown in above, the dictionary that had been attached to each LMFIT model
  is now moved to the ``data_subfit_map`` option.


.. _Release Notes_0.5.0_Other Notes:

Other Notes
-----------

.. releasenotes/notes/0.5/docs-refactoring-9f46f6539f57e8bd.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- The package documentation has been updated with introductory tutorials and how-tos
  for solving specific problems. It is now refactored into four sections: learning
  tutorials, how-to guides, experiment manuals, and the API references.

.. releasenotes/notes/0.5/docs-refactoring-9f46f6539f57e8bd.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- The internal curve analysis helper functions in ``curve_analysis.data_processing``
  have been moved to ``curve_analysis.utils``.

.. releasenotes/notes/0.5/ecr_lib-381cb18885e81abd.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- The :class:`.Calibrations` class has been updated to use the reference
  mechanism in Qiskit Pulse in which a schedule can refer to another
  schedule only by name.

.. releasenotes/notes/0.5/fix-drag-reanalysis-46f4c6679555242d.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- An analysis option ``reps`` in :class:`.DragCalAnalysis` was dropped. Now analysis
  is bootstrapped with circuit metadata and setting this value no longer impacts the
  analysis result. This upgrade doesn't introduce any breaking API change for existing
  experiments.

.. releasenotes/notes/0.5/rb_using_transpiled_cliffords-cd1376000a2379c4.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Improved the performance of circuit generation in 1Q/2Q randomized benchmarking experiments (about 10x speedup).
  That is mainly achieved by the following two updates in their implementation:

  * Custom transpilation of circuits (mapping circuits to physical qubits without using transpile),

  * Integer-based Clifford operations (especially sparse lookup table with triplet decomposition
    for 2Q Clifford circuits).

.. releasenotes/notes/0.5/suppress-runtime-warning-a741dc96f6a0ce7a.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- NumPy runtime warning for zero division has been suppressed in :class:`.CurveAnalysis`.
  This warning could occur in the edge case where the experiment data
  may contain data point with zero uncertainty.
  Such data point is safely ignored by LMFIT, since it may apply infinite fit weight.
  This runtime warning suppression makes standard error cleaner.

.. releasenotes/notes/0.5/update-number-to-2q-clifford-mapping-c28f1f29b0205d57.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- :meth:`.CliffordUtils.clifford_2_qubit` (and :meth:`.CliffordUtils.clifford_2_qubit_circuit`)
  changed its mapping between integers and 2Q Cliffords.
  As a consequence, circuits sampled by 2Q RB experiments may have been changed,
  even if exactly the same arguments are used for their construction.

.. releasenotes/notes/0.5/update-number-to-2q-clifford-mapping-c28f1f29b0205d57.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Removed unnecessary ``Barrier`` instructions in front of circuits generated by
  :class:`.StandardRB` and :class:`.InterleavedRB`.


.. _Release Notes_0.4.0:

0.4.0
=====

.. _Release Notes_0.4.0_Prelude:

Prelude
-------

.. releasenotes/notes/0.4/0_4_release-5716aa7442b995b2.yaml @ b'e6636bee289005debdd3f9bfde6455fc7b42cf38'

The Qiskit Experiments 0.4 release includes major improvements to the :class:`.CurveAnalysis` class and other bug fixes and improvements. The database service has switched to the `qiskit-ibm-experiment <https://github.com/Qiskit/qiskit-ibm-experiment>`_ provider. Several new data processing nodes have been added.


.. _Release Notes_0.4.0_New Features:

New Features
------------

.. releasenotes/notes/0.4/backend_data_class-270cec767b463e97.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Added a new class, :class:`.BackendData`, which provides a common access inferface
  for both :class:`~qiskit.providers.backend.BackendV1` and
  :class:`~qiskit.providers.backend.BackendV2` data fields, since those
  classes do not share the same interface. The :class:`.BackendData` can be called
  on a backend and used immediately, and it is also automatically stored as the
  ``_backend_data`` field of :class:`.BaseExperiment`. Note that not all data fields
  are currently accessible via :class:`.BackendData`; to access additional
  fields, the corresponding method should be added to :class:`.BackendData`
  with correct treatment for both V1 and V2 backends.

.. releasenotes/notes/0.4/curve-analysis-02a702a81e014adf.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- The :class:`.CurveAnalysis` class has been modified to delegate the core fitting functionality to the
  `LMFIT <https://lmfit.github.io/lmfit-py/>`_ package. Fit curves are specified using
  `LMFIT Model <https://lmfit.github.io/lmfit-py/model.html#>`_ objects. For multi-curve fitting
  a list of models can be used.

  A new analysis option ``fit_method`` has been added to allow a user to select the fitting
  algorithm used by the LMFIT `minimizer <https://lmfit.github.io/lmfit-py/fitting.html>`_.
  The default fit method is ``"least_squares"``.
  Analysis class author can flexibly define new analysis instance
  with LMFIT ``Model`` objects. See LMFIT documentation for user guide.

.. releasenotes/notes/0.4/curve-analysis-02a702a81e014adf.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- New curve analysis baseclass :class:`.CompositeCurveAnalysis` has been added.
  This curve analysis variant offers a framework to fit experiment outcomes
  with different independent fit models.
  For example, if you define an experiment scanning a parameter with different conditions,
  e.g. with different control qubit states in some two-qubit gate experiment,
  the composite curve analysis can implement the experiment with simpler code
  compared with writing a conventional batch experment.
  See class documentation for more details.

.. releasenotes/notes/0.4/curve-analysis-02a702a81e014adf.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- New options ``filter_data`` has been added to :class:`.CurveAnalysis` and its subclass.
  This dictionary provides a set of required metadata so that the analysis can filter
  experiment results input to the fitter. Curve analysis checks experiment result
  metadata, originating in the experiment circuit metadata, and the measure outcomes with
  matched metadata are only used for the fitting.

.. releasenotes/notes/0.4/curve-analysis-02a702a81e014adf.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- New options have been added to the :class:`.CurveAnalysis` curve drawer.

.. releasenotes/notes/0.4/curve-analysis-02a702a81e014adf.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- The ``figure_title`` option has been added. This allows user to show an arbitrary string
  in the output figure title. See the example code below to learn how to set the option.

  .. code-block:: python

    exp = MyExperiment(...)
    exp.analysis.drawer.set_options(figure_title="Qubit0")

.. releasenotes/notes/0.4/curve-analysis-02a702a81e014adf.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- ``plot_options`` has been added. This was conventionally included
  in the :class:`.SeriesDef` dataclass, which was static and not configurable.
  Now end-user can update visual representation of curves through this option.
  This option is a dictionary that defines three properties, for example,

  .. code-block:: python

    exp = MyExperiment(...)
    exp.analysis.drawer.set_options(
      plot_options={
        "curve1": {"color": "r", "symbol": "o", "canvas": 0},
        "curve2": {"color": "b", "symbol": "x", "canvas": 1},
      }
    )

  The dictionary is keyed on the curve name that should match with the ``name`` property
  of the LMFIT models provided to the curve analysis. ``color`` and ``symbol`` are the
  color and marker of the curves and the scatter plots, respectively.
  ``canvas`` specifies the sub-axis index, which is available when multi-axis plot is enabled.

.. releasenotes/notes/0.4/data-processor-e13a17d4c6b8dc99.yaml @ b'e6636bee289005debdd3f9bfde6455fc7b42cf38'

- A new data processing node :class:`.DiscriminatorNode` is added. This node
  wraps a pre-trained discriminator so that discrimination can be built
  into the data processing chain. The discriminator node is initialialized
  from a discriminator or list thereof which are objects that must have a
  predict method ``predict(x) -> y``  as is common in SKlearn. Here,
  :code:`x` is a list of IQ points and :code:`y` are the labels assigned to
  each point.

.. releasenotes/notes/0.4/data-processor-e13a17d4c6b8dc99.yaml @ b'e6636bee289005debdd3f9bfde6455fc7b42cf38'

- A new data processing node :class:`.RestlessToIQ` is added to
  process restless level one data, i.e., IQ data, in addition to the existing abstract
  class :class:`.RestlessNode` and :class:`.RestlessToCounts`
  for processing restless counts.


.. _Release Notes_0.4.0_Upgrade Notes:

Upgrade Notes
-------------

.. releasenotes/notes/0.4/0_3_deprecations-45cc3cbb0d175332.yaml @ b'e6636bee289005debdd3f9bfde6455fc7b42cf38'

- Several deprecated methods and options have been removed and will no longer work:

  * :meth:`.BaseExperiment.set_analysis_options` has been removed and should be
    replaced with :meth:`.BaseAnalysis.set_options`.
  * The ``curve_plotter`` option for :meth:`.CurveAnalysis.set_options` has been
    removed and replaced with ``curve_drawer``.
  * The ``curve_fitter`` option for :meth:`.CurveAnalysis.set_options` has been
    removed, now you can directly override :meth:`~CurveAnalysis._run_curve_fit`
    instead.
  * Setting ``style`` and drawer options with :meth:`.CurveAnalysis.set_options`
    has been disabled. Analyses should use ``drawer.set_options`` instead.
  * The ``FitVal`` class has been removed and replaced with the uncertainties package.
  * Boolean values for the analysis kwarg in :meth:`.BaseExperiment.run` have
    been disabled. Use ``analysis=default`` instead of ``analysis=True``, and
    ``analysis=None`` instead of ``analysis=False``.
  * :meth:`.BaseExperiment.run_analysis` has been removed. Use
    :meth:`.BaseAnalysis.run` instead.
  * :meth:`.BaseExperiment._postprocess_transpiled_circuits` is removed. Use
    :meth:`.BaseExperiment._transpiled_circuits` instead.
  * :meth:`.BaseExperiment.set_analysis_options` method has been deprecated, use
    the :meth:`.BaseAnalysis.set_options` method for the experiments analysis
    class instead.
  * The ``timeout`` kwarg of :meth:`.ExperimentData.add_data` has been removed.
    Timeout for adding jobs is now handled by the :meth:`.ExperimentData.add_jobs`
    method.
  * Adding data from jobs using :meth:`.ExperimentData.add_data` has been
    disabled. This method should now only be used to add data from Qiskit
    :class:`~qiskit.result` objects or raw data dicts. Job data should now be added
    using :meth:`.ExperimentData.add_jobs` instead.

.. releasenotes/notes/0.4/curve-analysis-02a702a81e014adf.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- A new result class :class:`.CurveFitResult` is introduced.
  This class stores a richer context of curve fitting overview with several extra statistics.
  This is the minimum attributes of the LMFIT ``MinimizerResult`` with some extention.
  Fit parameters in UFloat representation are also stored while keeping
  the correlation information, which is accessible with the ``.ufloat_params`` property.
  Note that the value of the first analysis result entry titled with ``@Parameters_*``
  has been replaced with this data format. This entry had been just a list of fit values
  in Python float format with covariance matrix separately stored in ``.extra`` metadata.
  Comparing with the conventional data, new class :class:`.CurveFitResult` provides users with
  a better understanding of the analysis outcome. New object has prettyprint mechanism.

.. releasenotes/notes/0.4/curve-analysis-02a702a81e014adf.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- The :class:`~.library.characterization.RoughDrag` characterization experiment has been upgraded with more flexibility.
  This experiment combines multiple DRAG parameter scans (curves) for different unit sequence
  reptitions. Conventionally this number is limited to three curves, however, now we can define
  more than three curves. The corresponding fit model is dynamically generated
  based on new fit option ``reps`` in the associated analysis :class:`~.library.characterization.DragCalAnalysis`.
  This may sometimes provide better accuracy for estimating the DRAG ``beta`` parameter.

.. releasenotes/notes/0.4/curve-analysis-02a702a81e014adf.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- The implementations of several methods in :class:`.BaseCurveAnalysis` have been moved to
  its subclass :class:`.CurveAnalysis`. :meth:`.BaseCurveAnalysis._run_data_processing`,
  :meth:`.BaseCurveAnalysis._format_data`, and :meth:`.BaseCurveAnalysis._run_curve_fit`
  have been turned into abstract methods, and :meth:`.BaseCurveAnalysis._generate_fit_guesses`
  has been moved to :class:`CurveAnalysis`. There is no net upgrade on the behavior of
  curve analysis subclasses, since :class:`.BaseCurveAnalysis` is an abstract class.

.. releasenotes/notes/0.4/experiment_data_refactor-1bb5ba366fb09bc5.yaml @ b'e6636bee289005debdd3f9bfde6455fc7b42cf38'

- The handling of communication with the database has been transferred to the
  new `qiskit-ibm-experiment <https://github.com/Qiskit/qiskit-ibm-experiment>`_
  package and does not rely on the soon to be deprecated
  `qiskit-ibmq-provider <https://github.com/Qiskit/qiskit-ibmq-provider>`_ package.
  In addition, The :class:`.ExperimentData` and
  :class:`.DbExperimentData` classes were merged, and the inner handling of experiment
  data was somewhat simplified. This should not have any effect on the current
  codebase and its usage of :class:`.ExperimentData`.

.. releasenotes/notes/0.4/figure_data-ecf5a82c95844b6a.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Added a :class:`.FigureData` class for adding metadata to analysis result figures. Figures added to
  :class:`.ExperimentData` are now stored using this class. The raw image object (SVG or matplotlib.Figure)
  can be accessed using the :attr:`.FigureData.figure` attribute.

  Note that currently metadata is only stored locally and will be discarded when saved to the cloud
  experiment service database.

.. releasenotes/notes/0.4/remove-DumpedOscillationAnalysis-c8eeb70bcc70e12a.yaml @ b'e6636bee289005debdd3f9bfde6455fc7b42cf38'

- The ``DumpedOscillationAnalysis`` class has been deprecated and will be removed soon. Use
  the :class:`.DampedOscillationAnalysis` class going forward.


.. _Release Notes_0.4.0_Deprecation Notes:

Deprecation Notes
-----------------

.. releasenotes/notes/0.4/curve-analysis-02a702a81e014adf.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Multiple methods, classes and functions in curve analysis have been deprecated and
  replaced with using functionality of the LMFIT library. These include:

  * Curve fit solver :func:`.curve_fit` and :func:`.multi_curve_fit`
  * Dataclass of the curve fit result :func:`.FitData`
  * Some fit functions dedicated to a particular curve analysis in the module
    :mod:`~qiskit_experiments.curve_analysis.fit_function`.
    Now curve analysis author can define arbitrary fit functions callable or string
    with LMFIT models, not limited to functions in this module.

.. releasenotes/notes/0.4/tomography-fitters-4a12b2ca9dee2625.yaml @ b'e6636bee289005debdd3f9bfde6455fc7b42cf38'

- The ``scipy_linear_lstsq`` and ``scipy_gaussian_lstsq`` fitters for the
  :class:`.StateTomographyAnalysis` and :class:`.ProcessTomographyAnalysis`
  classes have been deprecated.

  The unweighted, unconstrained least-squares fitting performed by
  ``scipy_linear_lstsq`` is equivalent to the :func:`.linear_inversion`
  fitter, but with worse performance and memory usage.

  For weighted least-squares fitting the CVXPY fitters
  :func:`.cvxpy_linear_lstsq` or :func:`.cvxpy_gaussian_lstsq`, which also
  support support PSD and CPTP constraints, should be used instead.


.. _Release Notes_0.4.0_Bug Fixes:

Bug Fixes
---------

.. releasenotes/notes/0.4/after-job-fail-aaaa0de4f28ae40c.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Fixed a bug that caused analysis to sometimes run after job or previous analysis failure.

.. releasenotes/notes/0.4/curve-analysis-02a702a81e014adf.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- The ``.init_params`` value of the :class:`.CurveFitResult` has been fixed.
  This value was copied from the LMFIT ``MinimizerResult.init_values``,
  however this is not the initial parameters set to the solver.
  Now correct initial parameters are set to :attr:`.CurveFitResult.init_params`.

.. releasenotes/notes/0.4/fix-analysis-result-copy-failing-22b3aa3a9fef18f2.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Fixed a bug when copying :class:`.AnalysisResult` (and thus copying :class:`.ExperimentData` and
  re-running some analyses) where an exception, regarding the ``extra``
  attribute of the :class:`.AnalysisResult` instance, would be thrown.

.. releasenotes/notes/0.4/fix-multi-series-plot-ac5ff39cabf5d578.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Fixed broken curve analysis output figure when multi canvas mode is enabled.
  Currently this feature is only used by :class:`.CrossResonanceHamiltonianAnalysis`.
  It has been plotting all series data in the same canvas due to the bug.

.. releasenotes/notes/0.4/fix_composite_data_setting-6fe361e91d5625e2.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Fixed a bug with the way composite experiments set the ``experiment_id``
  and ``experiment_type`` of :class:`.AnalysisResult` and :class:`.ExperimentData`.

.. releasenotes/notes/0.4/randomized_benchmarking-de55fda43765c34c.yaml @ b'eca6ce2a9c64f0b0b02d7434acf44e299c361613'

- Initial guess function for the randomized benchmarking analysis
  :func:`~.guess.rb_decay` has been upgraded to give accurate estimate of the decay
  function base.

.. releasenotes/notes/0.4/restless_enable_option-3486b0b0d89c1cd7.yaml @ b'94531e620a1df41efa7bc105f4c5e50405686725'

- The :meth:`~.RestlessMixin.enable_restless` method of the :class:`.RestlessMixin` class now has
  the non-default option to supress errors when T1 values are lower than
  the repetition dely. This allows users to accomodate cases when backends
  report erronous T1 values.

.. releasenotes/notes/0.4/timing-constraints-3b41f024c0f1b37e.yaml @ b'e6636bee289005debdd3f9bfde6455fc7b42cf38'

- Do not adjust timing constraints in experiments :class:`.T1`,
  :class:`.T2Hahn`, :class:`.T2Ramsey`, and :class:`.RamseyXY`. This
  adjustment was needed to supply missing timing constraints information for
  IBM backends but is not needed now and can lead to problems (see `#881
  <https://github.com/Qiskit/qiskit-experiments/issues/881>`_).


.. _Release Notes_0.3.1:

0.3.1
=====

New Features
------------

.. releasenotes/notes/0.3/0_3_1_release-43f09573952ce3ee.yaml @ b'7be5697f22e78842c961ddf70e478ebe8c3de59a'

- The class :class:`~MockIQBackend` for testing has been updated to support
  multiple qubits. It now takes lists of IQ cluster centers and widths as input,
  and specific backends are now defined as subclasses of
  :class:`.MockIQExperimentHelper`.


.. _Release Notes_0.3.1_stable_0.3_Bug Fixes:

Bug Fixes
---------

.. releasenotes/notes/0.3/0_3_1_release-43f09573952ce3ee.yaml @ b'7be5697f22e78842c961ddf70e478ebe8c3de59a'

- Fixes a bug where instantiating the
  :class:`.CrossResonanceHamiltonian` experiment without specifying the
  ``cr_gate`` and ``backend`` init kwargs raises an exception.

.. releasenotes/notes/0.3/0_3_1_release-43f09573952ce3ee.yaml @ b'7be5697f22e78842c961ddf70e478ebe8c3de59a'

- Fixes a visualization error in the figure produced during analysis of
  :class:`.CrossResonanceHamiltonian` when multi-canvas plotting mode is enabled.

.. releasenotes/notes/0.3/0_3_1_release-43f09573952ce3ee.yaml @ b'7be5697f22e78842c961ddf70e478ebe8c3de59a'

- Fixes an issue with marginalization of kerneled and discriminated memory in
  :class:`.CompositeAnalysis` not working correctly. This fixes
  :class:`.ParallelExperiments` not working correctly for level-1 measurement
  data.

.. releasenotes/notes/0.3/0_3_1_release-43f09573952ce3ee.yaml @ b'7be5697f22e78842c961ddf70e478ebe8c3de59a'

- Fixes a bug with JSON serialization of :class:`.ExperimentData` due to
  Qiskit ``backend`` and ``service`` objects not being JSON serializable. These
  properties are now set to ``None`` in the serialized experiment data.

.. _Release Notes_0.3.0:

0.3.0
=====

.. _Release Notes_0.3.0_Prelude:

Prelude
-------

.. releasenotes/notes/0.3/0_3_release-ba3ac7fef95aa042.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

The Qiskit Experiments 0.3 release includes several bug fixes and improvements and several new experiments added to the :mod:`~qiskit_experiments.library`. Added experiments include readout error, resonator spectroscopy,  two-qubit fine amplitude, and several characterization experiments. Experiments can  now be run restlessly without resetting.
There are also numerous changes and improvements to the :class:`.BaseExperiment`,   :class:`.ExperimentData`, :class:`.CurveAnalysis`, and composite experiment classes to improve JSON serialization,  handling of metadata, accessing the status of jobs and experiments, and the  storing and loading of experiments to and from the IBM experiment database service.


.. _Release Notes_0.3.0_New Features:

New Features
------------

.. releasenotes/notes/0.3/0_3_release-ba3ac7fef95aa042.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Added an ``analysis`` kwarg to :class:`.CompositeExperiment`, :class:`.BatchExperiment`
  and :class:`.ParallelExperiment` to allow a user to supply a custom
  :class:`.CompositeAnalysis` instance.

.. releasenotes/notes/0.3/cleanup-rb-experiment-f17b6e674ae4e473.yaml @ b'eca6ce2a9c64f0b0b02d7434acf44e299c361613'

- The curve fit parameter guess function :func:`~.guess.rb_decay` has been added.
  This improves the initial parameter estimation of randomized benchmark experiments.

.. releasenotes/notes/0.3/composite-analysis-c3119d5d2e64ce78.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Added :meth:`.CompositeAnalysis.component_analysis` method for accessing
  a component analysis class object from a composite analysis object.

.. releasenotes/notes/0.3/composite-combine-results-7c07820d99bd1b72.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Added a ``flatten_results`` init kwarg to :class:`.CompositeAnalysis`,
  :class:`.CompositeExperiment`, :class:`.ParallelExperiment`, and
  :class:`.BatchExperiment` that if set to ``True`` flattens all analysis
  results and figures from component experiment analysis into the main
  :class:`.ExperimentData` container, and does not save the individual
  child data components.

  Note that for nested composite experiments setting ``flatten_results=True``
  will recursively set the same value for all component experiments that
  are also composite experiments.

.. releasenotes/notes/0.3/curve-analysis-drawer-instance-bcfa18570915db2c.yaml @ b'a03eb85832e5b10ff32e71e9725b307fd0f1ada4'

- :class:`MplCurveDrawer` has been added for curve analysis visualization.
  This class instance is JSON serializable with the experiment encoder and
  it implements public methods to draw analysis results in several different formats.
  Its instance is attached to :class:`CurveAnalysis` instance as an analysis options ``curve_plotter``.
  This class is a drop-in replacement of :class:`MplDrawSingleCanvas` and :class:`MplDrawMultiCanvasVstack`.
  This instance has dedicated drawing options.
  New option ``subplots``, which is a tuple of two integer representing ``(n_rows, n_cols)``,
  defines arbitrary 2D array subplots without using :class:`MplDrawMultiCanvasVstack`.

.. releasenotes/notes/0.3/curve-analysis-drawer-instance-bcfa18570915db2c.yaml @ b'a03eb85832e5b10ff32e71e9725b307fd0f1ada4'

- Drawing options are moved from :attr:`CurveAnalysis.options` to :attr:`MplCurveDrawer.options`.

.. releasenotes/notes/0.3/exp-finalize-b7ca0a139ad5f872.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Added a :meth:`.BaseExperiment._finalize` method to :class:`.BaseExperiment`
  which is after configuring any runtime options, backend, or analysis
  classes but before generation and execution of experiment
  circuits during :class:`.BaseExperiment.run`.

  This method is intended to be overridden in experiment subclasses if they
  need to configure any analysis or runtime options based on a combination
  of properties of the experiment, for example some combination of backend,
  experiment and run options.

.. releasenotes/notes/0.3/expdata-futures-87a2ff561375e22b.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Improved handling of job and analysis processes in :meth:`.ExperimentData`.
  Verbose logging information on execution of analysis callbacks in an
  experiment can enabled by setting the ``qiskit_experiments`` log level
  to ``DEBUG``.

.. releasenotes/notes/0.3/expdata-futures-87a2ff561375e22b.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Added :meth:`.ExperimentData.jobs` method for returning a list of
  Qiskit Jobs for a running or finished experiment.

.. releasenotes/notes/0.3/expdata-futures-87a2ff561375e22b.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Added :meth:`.ExperimentData.job_status` method for returning the status
  of Qiskit Job execution for an experiment. This returns a
  :class:`.JobStatus` enum class value.

.. releasenotes/notes/0.3/expdata-futures-87a2ff561375e22b.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Added :meth:`.ExperimentData.analysis_status` method for returning the status
  of analysis callbacks for an experiment. This returns a
  :class:`.AnalysisStatus` enum class value.

.. releasenotes/notes/0.3/expdata-futures-87a2ff561375e22b.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Added :meth:`.ExperimentData.cancel_analysis` method to allow cancelling
  pending analysis callbacks. Note that analysis callbacks that have already
  started running cannot be cancelled.

.. releasenotes/notes/0.3/expdata-futures-87a2ff561375e22b.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Added :meth:`.ExperimentData.cancel` to cancel both jobs and analysis.

.. releasenotes/notes/0.3/expdata-futures-87a2ff561375e22b.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Added :meth:`.ExperimentData.add_jobs` method for adding one or more Qiskit
  jobs to experiment data. This method takes an optional ``timeout`` kwarg that
  when used will automatically cancel all non-finished jobs that exceed the
  alloted time.

.. releasenotes/notes/0.3/expdata-futures-87a2ff561375e22b.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Added enum classes for experiment, job, and analysis status.

.. releasenotes/notes/0.3/fake-service-e8b22e1a3394c136.yaml @ b'a03eb85832e5b10ff32e71e9725b307fd0f1ada4'

- Exposed and expanded the class :class:`~qiskit_experiments.test.FakeService`.
  The fake service will allow enhanced testing of qiskit-experiments and external packages.

.. releasenotes/notes/0.3/fineZXamp-restless-e0dbed212676957f.yaml @ b'a03eb85832e5b10ff32e71e9725b307fd0f1ada4'

- A new mixin class :mod:`~qiskit_experiments.framework.RestlessMixin` is added
  that enables experiments to run in restless measurement mode, where the qubits
  are not reset after each measurement.

.. releasenotes/notes/0.3/fineZXamp-restless-e0dbed212676957f.yaml @ b'a03eb85832e5b10ff32e71e9725b307fd0f1ada4'

- A new data processing node to marginalize qubit counts is introduced. This
  node is, for instance, used in the data processing of a fine ZX amplitude
  experiment run with restless measurements.

.. releasenotes/notes/0.3/generalize-fine-amp-63dbf0d8af33fb1c.yaml @ b'a03eb85832e5b10ff32e71e9725b307fd0f1ada4'

- A new fine amplitude experiment for two qubits is added. This experiment
  accepts a two-qubit gate with on rotation angle as parameter.

.. releasenotes/notes/0.3/readout-mitigation-experiment-4ea5392ee955a54c.yaml @ b'a03eb85832e5b10ff32e71e9725b307fd0f1ada4'

- Added two new experiments (:class:`~qiskit_experiments.library.LocalReadoutError` and :class:`~qiskit_experiments.library.CorrelatedReadoutError`)
  for characterizing the readout error of devices.

.. releasenotes/notes/0.3/resonator-spectroscopy-89f790412838ba5b.yaml @ b'a03eb85832e5b10ff32e71e9725b307fd0f1ada4'

- Added a new experiment
  :py:class:`~qiskit_experiments.library.ResonatorSpectroscopy` to run spectroscopy
  on readout resonators. This is done by attaching a custom pulse-schedule to
  the measure instruction. Note that the resonator spectroscopy experiment may
  cause errors on backends that do not support circuit instructions with measurement
  schedules attached to them.

.. releasenotes/notes/0.3/resonator-spectroscopy-89f790412838ba5b.yaml @ b'a03eb85832e5b10ff32e71e9725b307fd0f1ada4'

- A new data processing node
  :py:class:`~qiskit_experiments.data_processing.nodes.ToAbs` is introduced to
  take the absolute value of IQ points. This node is needed to analyse readout
  resonator spectroscopy IQ data since it rotates around in the IQ plane but can
  also be used in other contexts.

.. releasenotes/notes/0.3/serialize-expdata-47ad38c94bf479e1.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Added support for JSON serialization of :class:`.ExperimentData` objects.
  These objects can be serialized using the :class:`.ExperimentEncoder`
  and :class:`.ExperimentDecoder` classes.

  Note that serialization of general experiment results requires that the
  individual option values and analysis result types are themselves JSON
  serializable using the encoder and decoder classes.

.. releasenotes/notes/0.3/serialize-expdata-47ad38c94bf479e1.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Added support for pickling :class:`.ExperimentData` objects using the
  Python ``pickle`` module.

.. releasenotes/notes/0.3/t2-hahn-experiment-84fb05d71b5ef250.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Added a :class:`~qiskit_experiments.library.characterization.T2Hahn`
  class for composing and running Hahn Echo experiment to estimate T2.

.. releasenotes/notes/0.3/t2-hahn-experiment-84fb05d71b5ef250.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Added a :class:`~qiskit_experiments.library.characterization.analysis.T2HahnAnalysis`
  class for analyzing experiment data from :class:`~qiskit_experiments.library.characterization.T2Hahn`.

.. releasenotes/notes/0.3/t2-hahn-experiment-84fb05d71b5ef250.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Added a :class:`~qiskit_experiments.test.T2HahnBackend` class for testing
  which simulates T2 noise statistics.

.. releasenotes/notes/0.3/tomo-bases-e702b4094d717047.yaml @ b'a03eb85832e5b10ff32e71e9725b307fd0f1ada4'

- Added new tomography basis classes :class:`.LocalPreparationBasis` and
  :class:`.LocalMeasurementBasis` for constructing N-qubit bases from the
  tensor product of 1-qubit instructions. These classes can optionally be
  initialized with custom qubit-specific density matrix or POVM element
  states for respectively for tomographic reconstruction.

.. releasenotes/notes/0.3/tphi-757e15fcb24219f9.yaml @ b'a03eb85832e5b10ff32e71e9725b307fd0f1ada4'

- Added a new experiment Tphi. It computes the pure dephasing time and is computed as
  :math:`1/T_\varphi = 1/T_{2*} - 1/2T_1`.
  It is implemented as a composite experiment with sub-experiments
  T1 and T2Ramsey. The new classes are:
  :class:`~qiskit_experiments.library.characterization.Tphi` - class defining the Tphi
  experiment.

  :class:`~qiskit_experiments.library.characterization.analysis.TphiAnalysis` - class
  for Tphi analysis.

  :class:`~qiskit_experiments.test.TphiBackend` - fake backend for running a Tphi
  experiment, based on the fake backends for T1 and T2Ramsey.

.. releasenotes/notes/0.3/transpile-617bd3a4e6f1c0d8.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Added :meth:`.BaseExperiment._transpiled_circuits` which returns a list of
  experiment circuits, transpiled. It can be overridden to define custom
  transpilation.

.. releasenotes/notes/0.3/upgrade-curve-fit-4dc01b1db55ee398.yaml @ b'eca6ce2a9c64f0b0b02d7434acf44e299c361613'

- The all curve fit plot generated by the :class:`CurveAnalysis` shows a
  confidence interval properly computed with error propagation.
  By default it shows 1 sigma and 3 sigma region but you can customize this
  via the ``style`` option of the curve analysis subclass.
  The :class:`PlotterStyle` dataclass has been updated with new attribute :attr:`plot_sigma`
  which takes a list of ``tuple(float, float)`` specifying a pair of sigma and transparency.

.. releasenotes/notes/0.3/upgrade-serialize-data-processor-a3358b2a5e2fdc5b.yaml @ b'2008d3391ca10586c0c819c2474760322e20ec9a'

- :class:`~qiskit_experiments.data_processing.data_processor.DataProcessor`
  and :class:`~qiskit_experiments.data_processing.data_action.DataAction` are
  now JSON serializable with the qiskit experiments default encoder.
  This allows one to retrieve a configured processor from the record and
  re-analyze loaded data with the processor. Trained nodes are serialized with
  accquired parameters so that the loaded processor can continue to process new data.


.. _Release Notes_0.3.0_Upgrade Notes:

Upgrade Notes
-------------

.. releasenotes/notes/0.3/cleanup-cr-hamiltonian-experiment-7f47c51d26941f16.yaml @ b'2008d3391ca10586c0c819c2474760322e20ec9a'

- Experiment :class:`~qiskit_experiments.library.characterization.CrossResonanceHamiltonian`
  and its subclasses have been upgraded. Now its instance can generate circuits without
  setting backend for just checking experiment sequence. The sequence with actual parameters
  is generated after the backend is set. In addition, now experiments can take ``cr_gate``
  in the constractor which is ``Gate`` type subclass taking a single parameter (flat-top width).
  If one inputs a :class:`~qiskit.extensions.hamiltonian_gate.HamiltonianGate` subclass with
  cross resonance Hamiltonian, experiment can be simulated with Aer QASM simulator.

.. releasenotes/notes/0.3/cleanup-curve-analysis-96d7ff706cae5b4e.yaml @ b'a03eb85832e5b10ff32e71e9725b307fd0f1ada4'

- :class:`.BaseCurveAnalysis` class has been added as a superclass of :class:`.CurveAnalysis`.
  The new base class doesn't define the :meth:`_run_analysis` abstract method
  and it cannot conduct analysis by itself, however it defines several subroutines
  that can be combined to build a custom fitting process in the subclass.
  This allows more flexibility to write custom curve analysis by
  directly inheriting from the new base class. See :class:`.BaseCurveAnalysis` for details.
  See also `Issue 737 <https://github.com/Qiskit/qiskit-experiments/issues/737>`_ for discussion.

.. releasenotes/notes/0.3/cleanup-curve-analysis-96d7ff706cae5b4e.yaml @ b'a03eb85832e5b10ff32e71e9725b307fd0f1ada4'

- The method :meth:`CurveAnalysis._generate_fit_guesses` has been upgraded with
  a new method signature. Now this method is called with ``curve_data`` argument
  that provides dataset which is used for curve fitting.
  If you define custom :class:`.CurveAnalysis` subclass in your codestack,
  you may need to upgrade the method. See :class:`.BaseCurveAnalysis` for details.

.. releasenotes/notes/0.3/cleanup-curve-analysis-96d7ff706cae5b4e.yaml @ b'a03eb85832e5b10ff32e71e9725b307fd0f1ada4'

- Arguments of :class:`.FitData` have been updated to take ``x_data`` and ``y_data``
  instead of ``x_range`` and ``y_range``.

.. releasenotes/notes/0.3/cleanup-rb-experiment-f17b6e674ae4e473.yaml @ b'eca6ce2a9c64f0b0b02d7434acf44e299c361613'

- The computation of error per gates (EPGs) from EPC in :class:`RBAnalysis` has been upgraded.
  To compute these values from a single EPC value obtained by the experiment,
  we should provide a guess of contribution per basis gate to the depolarization.
  This ratio has been extracted from backend properties with
  :meth:`RBUtils.get_error_dict_from_backend`, but this approach may result in
  unreproducible EPG outcomes under certain circumstances.
  See `PR 762 <https://github.com/Qiskit/qiskit-experiments/pull/762>`_ for more details.
  Not this error ratio is provided from a hard-coded lookup table,
  and the user can still provide custom values with analysis option ``gate_error_ratio``.
  One can skip computation of EPGs by setting the option to ``False``.

.. releasenotes/notes/0.3/cleanup-rb-experiment-f17b6e674ae4e473.yaml @ b'eca6ce2a9c64f0b0b02d7434acf44e299c361613'

- :class:`RBAnalysis` has been upgraded to compute corrected EPC for 2Q RB.
  When the analysis option ``epg_1_qubit`` is provided,
  it returns two EPG analysis results, with and without correction for
  underlying single qubit depolarization channels.
  New result is added under the name ``EPC_corrected``.

.. releasenotes/notes/0.3/composite-analysis-c3119d5d2e64ce78.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- :class:`.CompositeAnalysis` initialization is changed to require a list of
  :class:`.BaseAnalysis` objects so that these are stored in the class, rather
  than being accessed later via a composite experiment. This initialization is
  handled automatically by :class:`.ParallelExperiment` and
  :class:`.BatchExperiment` composite experiments.

.. releasenotes/notes/0.3/composite-combine-results-7c07820d99bd1b72.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Changed the :class:`.Tphi` experiment and :class:`.TphiAnalysis` to combine
  the component analysis results so that it runs as a single experiment
  returning :math:`T_\phi`, :math:`T_1`, and :math:`T_2^\ast` analysis results.

.. releasenotes/notes/0.3/composite-exp-transpile-e37c257ba007ff40.yaml @ b'2008d3391ca10586c0c819c2474760322e20ec9a'

- The component experiment circuits of :class:`.ParallelExperiment` and
  :class:`.BatchExperiment` are now explicitly transpiled using the
  respective component experiments
  :meth:`~.BaseExperiment.transpile_options` before being combined into
  the composite circuits returned by the :class:`.BaseExperiment.circuits`
  method.

  Any transpile options set directly on the :class:`.ParallelExperiment`
  or :class:`.BatchExperiment` will also be applied as a transpile option
  to each component experiment.

.. releasenotes/notes/0.3/composite-exp-transpile-e37c257ba007ff40.yaml @ b'2008d3391ca10586c0c819c2474760322e20ec9a'

- The circuits returned by the :meth:`.ParallelExperiment.circuits` method
  of parallel circuits will now always be the combined circuits circuits
  of the transpiled circuits of the individual component experiments
  transpiled with that experiments transpile options.

.. releasenotes/notes/0.3/curve-analysis-fixed-parameters-5915a29db1e2628b.yaml @ b'eca6ce2a9c64f0b0b02d7434acf44e299c361613'

- New default :class:`CurveAnalysis` analysis option ``fixed_parameters``
  has been added. We can directly exclude parameters from the fit model
  of the particular analysis instance, rather than defining a new class to define
  the class attribute :attr:`CurveAnalysis.__fixed_parameters__`.

.. releasenotes/notes/0.3/data-processor-creation-8c399c4a4be9dd6b.yaml @ b'2008d3391ca10586c0c819c2474760322e20ec9a'

- The function :func:`get_processor` of the data processing package has an
  updated signature to make the method easy to extend. This will allow a
  more flexible creation of data processors.

.. releasenotes/notes/0.3/expdata-futures-87a2ff561375e22b.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- The value returned by :meth:`.ExperimentData.status` has been changed from
  a string to a :class:`.ExperimentStatus` enum class value.

.. releasenotes/notes/0.3/fix-tomography-fitter-b144c0df24c30d68.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- The signature of the ``outcome_data`` argument of the tomography
  fitter functions in :mod:`.library.tomography` has been changed from
  a list of NumPy ndarray vectors of non-zero observed frequencies into a
  single ndarray matrix containing the observed frequencies of all possible
  measurement outcomes for the measurement bases.

.. releasenotes/notes/0.3/generalize-fine-amp-63dbf0d8af33fb1c.yaml @ b'a03eb85832e5b10ff32e71e9725b307fd0f1ada4'

- The FineAmplitude experiment is refactored for more flexibility. Furthermore,
  the FineAmplitudeAnalysis class is upgraded to accept 0/1 calibration
  circuits to better fit the amplitude A of the ping-pong pattern.

.. releasenotes/notes/0.3/get-processor-multiple-qubits-dbf8767d22eadccc.yaml @ b'a03eb85832e5b10ff32e71e9725b307fd0f1ada4'

- The processing of ``meas_level=2`` data in the function :func:`get_processor` is generalized
  to data processing of experiments with more than one qubit.

.. releasenotes/notes/0.3/kids-in-analysis-df7b4dcbeb5b3125.yaml @ b'2008d3391ca10586c0c819c2474760322e20ec9a'

- The :class:`.ParallelExperiment` and :class:`.BatchExperiment` composite experiments
  have been changed to no longer return analysis results containing information about
  sub-experiments. Instead, use the :meth:`~.ParallelExperiment.child_data` method to
  retrieve sub-experiments of a given composite experiment.

.. releasenotes/notes/0.3/rb-data-processing-5433dc0257bb603e.yaml @ b'2008d3391ca10586c0c819c2474760322e20ec9a'

- The RB data processing flow is updated to align it with the recent
  refactoring of the :func:`get_processor` function. The RB analysis
  will now use the :func:`get_processor` function to choose the
  suitable data processor.

.. releasenotes/notes/0.3/remove-job-metadata-74ecfaa02f6182e1.yaml @ b'a03eb85832e5b10ff32e71e9725b307fd0f1ada4'

- The ``job_metadata`` field has been removed from
  :class:`.BaseExperiment`. Experiments which needed job metadata for
  analysis should now directly override the ``.BaseExperiment._metadata``
  method to store the required job metadata.

  Individual experiments using :class:`.CurveAnalysis` based analysis
  have been updated to store the ``meas_level`` and ``meas_return``
  run options in metadata if they have been set in the experiment for
  use in setting the data processor during analysis.

.. releasenotes/notes/0.3/remove-job-metadata-74ecfaa02f6182e1.yaml @ b'a03eb85832e5b10ff32e71e9725b307fd0f1ada4'

- The ``BaseExperiment._additional_metadata`` method has been removed, and
  experiments should now directly override the ``BaseExperiment._metadata``
  method to add additional experiment metadata to the run experiment data.

.. releasenotes/notes/0.3/tomo-bases-e702b4094d717047.yaml @ b'a03eb85832e5b10ff32e71e9725b307fd0f1ada4'

- The tomography experiment basis classes :class:`.PauliMeasurementBasis`,
  :class:`.PauliPreparationBasis`, and :class:`.Pauli6PreparationBasis` have
  been upgraded to be instances of the new tomography bases classes
  :class:`.LocalMeasurementBasis` and :class:`.LocalPreparationBasis`.

.. releasenotes/notes/0.3/tomo-bases-e702b4094d717047.yaml @ b'a03eb85832e5b10ff32e71e9725b307fd0f1ada4'

- Changed the signature of the :meth:`~.MeasurementBasis.circuit` and
  :meth:`~.MeasurementBasis.matrix` methods of tomography basis classes
  to require a ``qubits`` kwarg for specifying the specific physical qubits
  that the basis is being applied to.

.. releasenotes/notes/0.3/upgrade-curve-fit-4dc01b1db55ee398.yaml @ b'eca6ce2a9c64f0b0b02d7434acf44e299c361613'

- The :class:`.CurveAnalysis` class has been updated to use the covariance between fit
  parameters in the error propagation. This will provide more accurate standard
  error for your fit values.

.. releasenotes/notes/0.3/upgrade-curve-fit-4dc01b1db55ee398.yaml @ b'eca6ce2a9c64f0b0b02d7434acf44e299c361613'

- The data format of analysis result data value has been replaced from
  :class:`FitVal` to ``uncertainties.ufloat`` from the Python
  `uncertainties <https://pythonhosted.org/uncertainties/>`__ package to support
  error propagation for post analysis computation.

  .. code-block:: python

    expdata = T1(0, delays, backend).run()
    fit_t1 = expdata.analysis_results("T1").value

    assert isinstance(fit_t1, UFloat)

    new_value = fit_t1 / 2
    new_value.std_dev  # show new standard error value

  Now ``fit_t1`` value is an ``uncertainties.ufloat`` instance with
  new properties :attr:`.nominal_value` and :attr:`.std_dev`,
  and you can directly apply mathematical operation to this object.
  The new error value is predicted by linear error propagation theory.
  Note that you no longer need to separately compute the standard error.

  This computation is offered by the `uncertainties <https://pythonhosted.org/uncertainties/>`__
  package which is a requirement since from Qiskit Experiments v0.2.
  The functions supporting error propagation is also available in ``uncertainties.umath``.

.. releasenotes/notes/0.3/upgrade-curve-fit-4dc01b1db55ee398.yaml @ b'eca6ce2a9c64f0b0b02d7434acf44e299c361613'

- In the analysis result object, :attr:`FitVal.unit` property has been moved to
  :attr:`DbAnalysisResultV1.extra` as metadata.


.. _Release Notes_0.3.0_Deprecation Notes:

Deprecation Notes
-----------------

.. releasenotes/notes/0.3/cleanup-curve-analysis-96d7ff706cae5b4e.yaml @ b'a03eb85832e5b10ff32e71e9725b307fd0f1ada4'

- Several protected methods of :class:`.CurveAnalysis` intended to be overriden
  or directly be used by subclass have been deprecated.
  :meth:`CurveAnalysis._data` has been deprecated without alternative method
  to make :class:`.CurveAnalysis` state cleaner. Now relevent curve analysis methods
  requiring curve data are called with the ``curve_data`` argument.
  :meth:`CurveAnalysis._extra_database_entry` has also been deprecated.
  This method becomes a part of :meth:`CurveAnalysis._create_analysis_results`.
  Analysis class author can override this method to inject a code to create
  custom analysis results.

.. releasenotes/notes/0.3/cleanup-rb-experiment-f17b6e674ae4e473.yaml @ b'eca6ce2a9c64f0b0b02d7434acf44e299c361613'

- Calling :class:`RBUtils` methods have been deprecated and will be removed after 0.4.

.. releasenotes/notes/0.3/composite-analysis-c3119d5d2e64ce78.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- The :meth:`.CompositeExperiment.component_analysis` method has been
  deprecated. Component analysis classes should now be directly accessed
  from a :meth:`.CompositeAnalysis` object using the
  :meth:.`CompositeAnalysis.component_analysis` method.

.. releasenotes/notes/0.3/curve-analysis-drawer-instance-bcfa18570915db2c.yaml @ b'a03eb85832e5b10ff32e71e9725b307fd0f1ada4'

- Conventional curve visualization classes :class:`MplDrawSingleCanvas`,
  :class:`MplDrawMultiCanvasVstack` and the stylesheet :class:`PlotterStyle` have been deprecated
  and now replaced with :class:`MplCurveDrawer`.
  These classes had been attached to the analysis instance as a ``curve_plotter`` which is a string
  and mapped to the class method ``.draw`` at runtime via :FitResultPlotters: Enum.
  It was almost impossible to track the code and hurted the readability.
  In addition, this implementation was problematic due to dependency on the
  raw data points saved in an instance variable. See qiskit-experiments/#737 for details.

.. releasenotes/notes/0.3/curve-analysis-fixed-parameters-5915a29db1e2628b.yaml @ b'eca6ce2a9c64f0b0b02d7434acf44e299c361613'

- Class attribute :attr:`CurveAnalysis.__fixed_parameters__` has been deprecated
  and support for the instantiation of the class with this attribute will be dropped soon.
  In addition, the fixed parameter value defined as a standalone analysis option
  has been deprecated. Please set `fixed_parameters` option instead.
  This is a python dictionary of fixed parameter values keyed on the fit parameter names.

.. releasenotes/notes/0.3/curve-analysis-fixed-parameters-5915a29db1e2628b.yaml @ b'eca6ce2a9c64f0b0b02d7434acf44e299c361613'

- Analysis class ``FineDragAnalysis`` has been deprecated. Now you can directly
  set fixed parameters to the :class:`.ErrorAmplificationAnalysis` instance as an analysis option.

.. releasenotes/notes/0.3/curve-analysis-fixed-parameters-5915a29db1e2628b.yaml @ b'eca6ce2a9c64f0b0b02d7434acf44e299c361613'

- Analysis class ``FineFrequencyAnalysis`` has been deprecated. Now you can directly
  set fixed parameters to the :class:`.ErrorAmplificationAnalysis` instance as an analysis option.

.. releasenotes/notes/0.3/curve-analysis-fixed-parameters-5915a29db1e2628b.yaml @ b'eca6ce2a9c64f0b0b02d7434acf44e299c361613'

- Analysis class ``FineHalfAngleAnalysis`` has been deprecated. Now you can directly
  set fixed parameters to the :class:`.ErrorAmplificationAnalysis` instance as an analysis option.

.. releasenotes/notes/0.3/deprecate-cals-library-128909c1379330fe.yaml @ b'2008d3391ca10586c0c819c2474760322e20ec9a'

- The library argument to :class:`.Calibrations` has been deprecated in favour
  of a new argument called libraries.

.. releasenotes/notes/0.3/expdata-futures-87a2ff561375e22b.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Adding data from jobs using :meth:`.ExperimentData.add_data` has been
  deprecated. This method should now only be used to add data from Qiskit
  :class:`.Result` objects or raw data dicts.
  Job data should now be added using the new :meth:`.ExperimentData.add_jobs`
  method instead.

.. releasenotes/notes/0.3/expdata-futures-87a2ff561375e22b.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- The ``timeout`` kwarg of :meth:`.ExperimentData.add_data` has been deprecated.
  Timeout for adding jobs is now handled by the :meth:`.ExperimentData.add_jobs`
  method.

.. releasenotes/notes/0.3/transpile-617bd3a4e6f1c0d8.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- ``BaseExperiment._postprocess_transpiled_circuits`` is deprecated and
  will be removed in the 0.4.0 release.
  Use :meth:`.BaseExperiment._transpiled_circuits` instead.

.. releasenotes/notes/0.3/upgrade-curve-fit-4dc01b1db55ee398.yaml @ b'eca6ce2a9c64f0b0b02d7434acf44e299c361613'

- The :class:`FitVal` class had been deprecated and being replaced with the uncertainties package.
  When loading saved experiments or analysis results any :class:`FitVal` s will be
  implicitly converted into :class:`UFloat` which should be re-saved
  to ensure these experiments can be reloaded in the future.


.. _Release Notes_0.3.0_Bug Fixes:

Bug Fixes
---------

.. releasenotes/notes/0.3/add-delay-support-in-irb-ae090968aadd7a54.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Fixed a bug in the :class:`.InterleavedRB` experiment where a :class:`.Delay` instruction,
  or a Clifford circuit containing delay instructions, could not be used as the interleaved element.

.. releasenotes/notes/0.3/cvxpy-utils-10ad67668aea82fe.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Fixed built-in tomography fitter functions :func:`.cvxpy_linear_lstsq`,
  :func:`.cvxpy_gaussian_lstsq`,
  :func:`.scipy_linear_lstsq`,
  :func:`.scipy_gaussian_lstsq`,
  :func:`.linear_inversion` to make the ``measurement_basis`` kwarg
  optional so that these functions could be used for fitting raw tomography
  fitter data with preparation data but no measurement data.

.. releasenotes/notes/0.3/cvxpy-utils-10ad67668aea82fe.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Fixed bug in :class:`.TomographyAnalysis` when accumulating count data
  from repeated circuits using the same preparation and measurement basis
  configuration.

.. releasenotes/notes/0.3/expdata-futures-87a2ff561375e22b.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Fixed an issue with :meth:`.ExperimentData.block_for_results` sometimes
  having a race issue with all analysis callbacks finishing.

.. releasenotes/notes/0.3/experiment_service_fixes-94730fd6bab83956.yaml @ b'eca6ce2a9c64f0b0b02d7434acf44e299c361613'

- :meth:`.ExperimentData.save` should now fail gracefully when experiment metadata failed to save instead of crashing.

.. releasenotes/notes/0.3/experiment_service_fixes-94730fd6bab83956.yaml @ b'eca6ce2a9c64f0b0b02d7434acf44e299c361613'

- The link to the experiment entry in the database service shown after saving is now by default obtained from the service, not hard-coded.

.. releasenotes/notes/0.3/fineZXamp-restless-e0dbed212676957f.yaml @ b'a03eb85832e5b10ff32e71e9725b307fd0f1ada4'

- The FineZXAmplitude now works properly with restless measurements.

.. releasenotes/notes/0.3/fix-asymmetric-qpt-3107ef95e8c117c6.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Fixed a bug with the :class:`.ProcessTomography` where the default target
  channel analysis option was computed incorrectly if not all qubits were
  prepared and measured, and the preparations and measurements were applied
  to different subsets of qubits.

  See `Issue 758 <https://github.com/Qiskit/qiskit-experiments/issues/758>`_
  for details.

.. releasenotes/notes/0.3/fix-asymmetric-qpt-3107ef95e8c117c6.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Fixed a bug with the :class:`.ProcessTomographyAnalysis` where analysis
  would raise an exception if the number of prepared and measurement qubits
  are not equal.

  See `Issue 757 <https://github.com/Qiskit/qiskit-experiments/issues/757>`_
  for details.

.. releasenotes/notes/0.3/fix-composite-copy-a869e9773f6a4d48.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Fixed :meth:`.ParallelExperiment.copy` and :meth:`.BatchExperiment.copy`
  so that the copies preserves any references between the original
  component experiments analysis classes and the :class:`.CompositeAnalysis`
  classes component analysis classes.

.. releasenotes/notes/0.3/fix-composite-copy-a869e9773f6a4d48.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Fixed :meth:`.CompositeAnalysis.copy` to recursively make a copy of the
  component analysis classes.

.. releasenotes/notes/0.3/fix-decay-init-guess-22903624c6b7490e.yaml @ b'a03eb85832e5b10ff32e71e9725b307fd0f1ada4'

- The initial guess function :func:`exp_decay`, which estimates an exponent of the
  decay curve by taking the natural logarithm of the y values, has been updated to
  handle exceptions when some y values are negative.

.. releasenotes/notes/0.3/fix-json-main-bedf4b9b18c851ac.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Fixed a bug with JSON deserialization using the :class:`.ExperimentDecoder`
  failing to decode custom user classes defined in the ``__main__`` scope of
  python scripts and notebooks.

.. releasenotes/notes/0.3/fix-nested-comp-66a2b8b6e3b404be.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Fixed bug in :class:`.CompositeAnalysis` where analysis of nested
  composite experiments could raise a RuntimeError.

.. releasenotes/notes/0.3/fix-tomography-fitter-b144c0df24c30d68.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Fixed a bug in :class:`.TomographyAnalysis` where the basis elements of
  unobserved measurement outcomes were not being included in the fitter
  objective function for least-squares fitters (CVXPY and SciPy).
  This would lead to lower than expected fit fidelities when fitting data
  with many zero count outcomes (typically synthetic data from ideal simulation).

.. releasenotes/notes/0.3/fix-tomography-fitter-b144c0df24c30d68.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Fixed issue with the CVXPY :class:`.ProcessTomography` analysis fitter
  functions :func:`.cvxpy_linear_lstsq` and :func:`cvxpy_gaussian_lstsq`
  where the trace preserving constraint was not being applied to the fit
  functions by default and required being explicitly passed as a
  ``solver_option``. Now all CVXPY process tomography experiments will
  have this option set to True by default unless a user explicitly
  disables it by setting the solver_option to False.

.. releasenotes/notes/0.3/fixed-experiment-link-87a7059830c140e6.yaml @ b'2008d3391ca10586c0c819c2474760322e20ec9a'

- Cloud database experiment links no longer display when there was an error
  saving to the API.

.. releasenotes/notes/0.3/improve-cancel-a1e7b6dc331014cd.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Fixed some issues with :meth:`.ExperimentData.cancel_analysis`
  and :meth:`.ExperimentData.cancel` to make cancelling analysis
  callbacks more robust.

.. releasenotes/notes/0.3/improve-cancel-a1e7b6dc331014cd.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Fixed :meth:`.ExperimentData.block_for_results` to handle blocking
  in recursive cases where an analysis callback adds another
  job or analysis callback.

.. releasenotes/notes/0.3/resonator-spectroscopy-89f790412838ba5b.yaml @ b'a03eb85832e5b10ff32e71e9725b307fd0f1ada4'

- The ResonanceAnalysis class has been switched from a Gaussian fit to a Lorentzian
  fit function. Furthermore, the Gaussian fitting capability is preserved by moving
  the Gaussian fitting to a new class called GaussianAnalysis. Note that the
  previous analysis can be used by doing:

  .. code:: python

    spec = ResonatorSpectroscopy(qubit, backend)
    spec.analysis = GaussianAnalysis()

  where :code:`GaussianAnalysis` is imported from ``curve_analysis``.

.. releasenotes/notes/0.3/svd-node-fix-bec02332e1db96ec.yaml @ b'2008d3391ca10586c0c819c2474760322e20ec9a'

- A bug related to single-shot data in the SVD data processing node is fixed.

.. releasenotes/notes/0.3/upgrade-serialize-data-processor-a3358b2a5e2fdc5b.yaml @ b'2008d3391ca10586c0c819c2474760322e20ec9a'

- Poor python instance representation of
  :class:`~qiskit_experiments.data_processing.data_action.DataAction`
  has been upgraded to show all information contained in the class instance.


.. _Release Notes_0.3.0_API Changes for Experiment Authors:

API Changes for Experiment Authors
----------------------------------

.. releasenotes/notes/0.3/cvxpy-utils-10ad67668aea82fe.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Refactored some of the internal CVXPY code in
  ``qiskit_experiments.library.tomography.fitters.cvxpy_utils`` used by the
  CVXPY tomography fitters to make it easier to generate complex variable
  SDP optimization problems.

.. releasenotes/notes/0.3/developer_add_success_check-5ddd1d56be29a329.yaml @ b'a03eb85832e5b10ff32e71e9725b307fd0f1ada4'

- The :py:func:`assertExperimentDone` function has been added to
  :py:class:`test.base.QiskitExperimentsTestCase`. This assertion will check
  if all threads in the experiment data are successfuly completed.
  This function calls :meth:`block_for_results` and then checks if the experiment
  status returns ``DONE`` after execution. It is `highly recommended` to use this test
  right after each experiment execution to detect program malfunction,
  which is particularly relevant to python multi-threading in multi-platform.

.. releasenotes/notes/0.3/feature-warnings-helper-c44bfb654345f437.yaml @ b'2008d3391ca10586c0c819c2474760322e20ec9a'

- New module :mod:`qiskit_experiments.warnings` has been added.
  This module implements several decorator functions to raise user-friendly deprecation warning
  and some also patch the decorated logic to implements new logic for backport.
  See decorator function documentations for details.

.. releasenotes/notes/0.3/tomo-bases-e702b4094d717047.yaml @ b'a03eb85832e5b10ff32e71e9725b307fd0f1ada4'

- Streamlined the tomography experiment basis base classes into two
  abstract base classes :class:`.PreparationBasis` and
  :class:`.MeasurementBasis`.

.. releasenotes/notes/0.3/upgrade-serialize-data-processor-a3358b2a5e2fdc5b.yaml @ b'2008d3391ca10586c0c819c2474760322e20ec9a'

- Data handling of training parameters in
  :class:`~qiskit_experiments.data_processing.data_action.TrainableDataAction`
  has been upgraded for the JSON serialization.
  Updated class implements :meth:`_default_parameters`, :meth:`set_parameters`, and
  :meth:`parameters` methods, where the training parameters are managed with :class:`Options`
  instance. A node developer must implement :meth:`_default_parameters` class method
  to automatically populate the JSON configuration dictionary.


.. _Release Notes_0.3.0_Other Notes:

Other Notes
-----------

.. releasenotes/notes/0.3/fix-error-amp-analysis-bounds-784f3aa66d16048a.yaml @ b'2008d3391ca10586c0c819c2474760322e20ec9a'

- Default fit bounds for ``d_theta`` parameter of
  :py:class:`qiskit_experiments.curve_analysis.ErrorAmplificationAnalysis`
  class has been updated from [-pi, pi] to [-0.8 pi, 0.8 pi].
  This change will improve the bad fit when the error value is really close to zero.
  This has sometimes yielded in ``d_theta`` ~ pi rather than zero.
  Though 0.8 is the empirical factor, this is okey for most situations since
  the amplification analysis is applied to experiments in the small error regime
  (this is often sufficiently smaller than pi).


.. _Release Notes_0.2.0:

0.2.0
=====

.. _Release Notes_0.2.0_Prelude:

Prelude
-------

.. releasenotes/notes/0.2/0_2_release-eef5e3ba256fc750.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

The Qiskit Experiments 2.0 release includes several bug fixes and improvements and many new experiments added to the :mod:`~qiskit_experiments.library`. Added experiments include a full suite of single-qubit gate calibration and characterization experiments, and two-qubit Cross-resonance Hamiltonian characterization experiments.
There are also numerous changes and improvements to the base classes in :mod:`~qiskit_experiments.framework` and :mod:`~qiskit_experiments.calibration_management` to make developing new experiments easier, to improve JSON serialization, and to  improve storing and loading experiments for the IBM experiment database service.


.. _Release Notes_0.2.0_New Features:

New Features
------------

.. releasenotes/notes/0.2/0_2_release-eef5e3ba256fc750.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- You can now change the default ``matplotlib`` backend used for generated
  figures by setting
  ``qiskit_experiments.framework.matplotlib.default_figure_canvas`` to the
  desired canvas. Note that it has to be a canvas for one of the
  `non-interactive backend
  <https://matplotlib.org/stable/tutorials/introductory/usage.html#the-builtin-backends>`_.
  For example, you can set ``default_figure_canvas`` to
  :class:`~matplotlib.backends.backend_agg.FigureCanvasAgg` to use the
  ``AGG`` backend.

.. releasenotes/notes/0.2/base-analysis-b261afaa40518b53.yaml @ b'770a3dffd30d9093ec20ad85676f0b2f92393c4a'

- Added the ``replace_results`` kwarg to
  :meth:`~qiskit_experiments.framework.BaseAnalysis.run` with default
  value of ``replace_results=False``.

  If analysis is run with ``replace_results=True`` then any analysis results
  and figures in the experiment data will be cleared and replaced with the
  new analysis results. Saving this experiment data will replace any
  previously saved data in a database service using the same experiment ID.

  If analysis is run with ``replace_results=False`` and the experiment data
  being analyzed has already been saved to a database service, or already
  contains analysis results or figures, a copy with a unique experiment ID
  will be returned containing only the new analysis results and figures.
  This data can then be saved as its own experiment to a database service.

.. releasenotes/notes/0.2/base-analysis-b261afaa40518b53.yaml @ b'770a3dffd30d9093ec20ad85676f0b2f92393c4a'

- Added a :meth:`~qiskit_experiments.framework.BaseAnalysis.set_options`
  method and :meth:`~qiskit_experiments.framework.BaseAnalysis.options`
  property to the :class:`qiskit_experiments.framework.BaseAnalysis` class
  to store and retrieve any analysis options in the state of the analysis
  instance.

.. releasenotes/notes/0.2/base-experiment-14eba2646ef0f0b9.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- The ``analysis`` kwarg of
  :meth:`qiskit_experiments.framework.BaseExperiment.run` can now optionally
  be passed a :class:`qiskit_experiments.framework.BaseAnalysis` instance to
  use for analysis of that single execution. If no instance is provided the
  current stored :meth:`~qiskit_experiments.framework.BaseExperiment.analysis`
  instance for that experiment will be used. Setting ``analysis=None`` disables
  analysis for the specific execution.

.. releasenotes/notes/0.2/base-experiment-14eba2646ef0f0b9.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Added ``backend`` as an optional ``__init__`` kwarg for all experiments to
  allow setting the backend at initialization. The backand can also be set
  and retrieved from the experiment object after construction using the
  :meth:`~qiskit_experiments.framework.BaseExperiment.backend`
  property and setter.

  When using the ``backend`` kwarg of
  :meth:`~qiskit_experiments.framework.BaseExperiment.run` to specify
  a backend this will temporarily override any currently set backends
  for that single execution.

.. releasenotes/notes/0.2/base-experiment-14eba2646ef0f0b9.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Added :class:`~qiskit_experiments.framework.ExperimentConfig` dataclass
  for storing the configuration of an experiment. This configuration can
  be obtained by using the
  :meth:`~qiskit_experiments.framework.BaseExperiment.config` property.
  Experiments can also be reconstructed from their configuration using
  the :meth:`~qiskit_experiments.framework.BaseExperiment.from_config`
  class method.

.. releasenotes/notes/0.2/base-experiment-14eba2646ef0f0b9.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Added automatic job splitting to
  :class:`~qiskit_experiments.framework.BaseExperiment` for execution of
  experiments with a larger number of circuits than can be run in a single
  job on the target backend. This enables running large experiments on
  legacy and non-IBM backends that don't handle job splitting
  automatically.

.. releasenotes/notes/0.2/calibrations-97c6ae807d54015d.yaml @ b'770a3dffd30d9093ec20ad85676f0b2f92393c4a'

- Added support for JSON serialization to
  :class:`qiskit_experiments.calibration_management.BasisGateLibrary`.

.. releasenotes/notes/0.2/calibrations-97c6ae807d54015d.yaml @ b'770a3dffd30d9093ec20ad85676f0b2f92393c4a'

- Simplified the update library for calibration experiments by merging
  this functionality into the
  :class:`qiskit_experiments.calibration_management.BaseCalibrationExperiment`.

  Future releases may fully deprecate the
  :class:`qiskit_experiments.calibration_management.update_library.BaseUpdater`
  in favour of moving its functionality into the
  :class:`~qiskit_experiments.calibration_management.BaseCalibrationExperiment`.

.. releasenotes/notes/0.2/data-processors-d6430844d2701eb1.yaml @ b'a03eb85832e5b10ff32e71e9725b307fd0f1ada4'

- The :class:`qiskit_experiments.data_processing.Probability`
  data processing node has been enhanced to compute the estimated mean and
  standard deviation of a measured outcome probability using a Bayesian update
  of a a Beta distribution prior from the observed measurement outcomes.
  The default prior is an uninformative prior. The user can also provide a custom
  prior for the probability distribution.

.. releasenotes/notes/0.2/experiment-data-5465208160fe6b6a.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Added
  :meth:`~qiskit_experiments.database_service.DbExperimentData.add_analysis_callback`
  method to :class:`~qiskit_experiments.framework.ExperimentData`
  for adding a post-processing analysis function to run as a callback after
  currently executing experiment jobs are finished.

.. releasenotes/notes/0.2/experiment-data-5465208160fe6b6a.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Added a ``block`` kwarg with default value ``block=True`` to the
  :meth:`qiskit_experiments.framework.ExperimentData.analysis_results`
  method. If this is True then calling
  :meth:`~qiskit_experiments.framework.ExperimentData.analysis_results`
  will block to wait for all running analysis callbacks to finish before
  returning results. This prevents issues where trying to retrieve analysis
  results before analysis was finished would raise an error that the result
  could not be found.

  Note that in the case of
  :class:`~qiskit-experiments.framework.ParallelExperiment` and
  :class:`~qiskit-experiments.framework.BatchExperiment` blocking or
  calling ``analysis_results`` on the parent experiment should be performed
  before attempting to access results in the component experiment data
  containers to ensure the component analysis callbacks
  have been initialized.

.. releasenotes/notes/0.2/experiment-data-5465208160fe6b6a.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- The :class:`~qiskit_experiments.framework.ExperimentData` class
  can now store child ``ExperimentData`` containers.
  Child data can either be added at initialization using the
  ``child_data`` kwarg or added later using the
  :meth:`~qiskit_experiments.framework.ExperimentData.add_child_data`
  method. Child ``ExperimentData`` can be accessed using the
  :meth:`~qiskit_experiments.framework.ExperimentData.child_data`
  method.

.. releasenotes/notes/0.2/experiment-data-5465208160fe6b6a.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Added a :meth:`~qiskit_experiments.framework.ExperimentData.copy`
  method to :class:`~qiskit_experiments.framework.ExperimentData` which
  allows making a copy of an experiment data container with a new
  experiment ID, new result IDs, and new figure names, generated for
  the copy.

  This method has a kwarg option ``copy_results`` that can be set to
  ``False`` to only copy the experiment
  :meth:`~qiskit_experiments.framework.ExperimentData.data` and
  metadata, but not the analysis results and figures.

.. releasenotes/notes/0.2/experiment-data-5465208160fe6b6a.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Added methods
  :meth:`~qiskit_experiments.framework.ExperimentData.add_tags_recursive` and
  :meth:`~qiskit_experiments.framework.ExperimentData.remove_tags_recursive`
  to :class:`qiskit_experiments.framework.ExperimentData` for adding and
  removing tags of an experiment data object and all its
  :meth:`~qiskit_experiments.framework.ExperimentData.child_data`.

.. releasenotes/notes/0.2/experiment-data-5465208160fe6b6a.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Added support for saving and loading
  :class:`qiskit_experiments.framework.ParallelExperiment`
  and :class:`qiskit_experiments.framework.BatchExperiment` experiment data
  and all component experiment data and results from the IBM experiments
  database service. Changing the share level of the parent composite
  experiment will also change the share level of all component experiments.

  When saving composite experiments each component experiment analysis
  results and figures will be saved under a unique experiment ID. Note that
  these component experiments do not save any of the marginalized circuit
  result data. The unmarginalized circuit result data is saved in the parent
  componsite experiments.

.. releasenotes/notes/0.2/library-26e6cf3dfbc3acb3.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Added a collection of experiments for performing single-qubit gate
  :mod:`~qiskit_experiments.library.characterization` and
  :mod:`~qiskit_experiments.library.calibration`. The new experiments are

  * :class:`~qiskit_experiments.library.characterization.Rabi`:
    This experiment scans the amplitude of a pulse and measures the qubit
    population. This allows us to determine the amplitude that creates,
    for example, an ``X`` gate and/or a ``SX`` gate.

  * :class:`~qiskit_experiments.library.calibration.RoughXSXAmplitudeCal`:
    The calibration version of :class:`~qiskit_experiments.library.characterization.Rabi`.
    It extracts the amplitudes needed to implement an ``X`` gate an a ``SX`` gate.
    This type of calibration is a rough amplitude calibration since the resulting
    parameter value is typically not very precises.

  * :class:`~qiskit_experiments.library.characterization.FineAmplitude`: This experiment repeats
    a rotation a variable number of times to amplify over- and under-rotations.
    The resulting ping-pong pattern in the qubit population is fit to determine
    the error in the rotation angle.
    This experiment has specializations for X
    (:class:`~qiskit_experiments.library.characterization.FineXAmplitude`) and SX
    (:class:`~qiskit_experiments.library.characterization.FineSXAmplitude`) gates.

  * :class:`~qiskit_experiments.library.calibration.FineAmplitudeCal`: The calibration version
    of :class:`~qiskit_experiments.library.characterization.FineAmplitude`.
    It will update the amplitude of the pulse according to the measurred deviation.
    This experiment has specializations for X
    (:class:`~qiskit_experiments.library.calibration.FineXAmplitudeCal`) and SX
    (:class:`~qiskit_experiments.library.calibration.FineSXAmplitudeCal`) gates.

  * :class:`~qiskit_experiments.library.characterization.RoughDrag`:
    This experiment scans the DRAG parameter of a repeated
    sequence of rotation and anti-rotation. If the DRAG parameter does not have
    the correct value phase errors will accumulate and the repeated sequece of
    gates will not return the qubit to the ground state.

  * :class:`~qiskit_experiments.library.calibration.RoughDragCal`: The calibration version of
    :class:`~qiskit_experiments.library.characterization.RoughDrag`.

  * :class:`~qiskit_experiments.library.characterization.FineDrag`: This experiment iterates the
    gate sequence Rp - Rm where Rp is a rotation around an axis and Rm is the same
    rotation but in the opposite direction. This sequence amplifies phase errors due
    to the presence of higher excited states.
    This experiment has specializations for X
    (:class:`~qiskit_experiments.library.characterization.FineXDrag`) and SX
    (:class:`~qiskit_experiments.library.characterization.FineSXDrag`) gates.

  * :class:`~qiskit_experiments.library.calibration.FineDragCal`: The calibration version of
    :class:`~qiskit_experiments.library.characterization.FineDrag`.
    This will update the DRAG parameter in the instance of the
    :class:`:class:`~qiskit_experiments.calibration_management.Calibrations` class.
    This experiment has specializations for X
    (:class:`~qiskit_experiments.library.calibration.FineXDragCal`) and SX
    (:class:`~qiskit_experiments.library.calibration.FineSXDragCal`) gates.

  * :class:`~qiskit_experiments.library.characterization.QubitSpectroscopy`: This experiment
    performs spectroscopy by applying a frequency shift to a long pulse. This experiment
    is typically used to identify the resonance frequency of the qubit.

  * :class:`~qiskit_experiments.library.calibration.RoughFrequencyCal` the calibration
    version of :class:`~qiskit_experiments.library.characterization.QubitSpectroscopy`.
    This gives us a first rough estimate of the qubits frequency.

  * :class:`~qiskit_experiments.library.characterization.RamseyXY`: This experiment
    performs a Ramsey-XY experiment which allows us to measure the frequency of the qubit.
    This experiment is sensitive to the sign of the frequency offset from the main transition.
    It is a more precise measurement than spectroscopy.

  * :class:`~qiskit_experiments.library.calibration.FrequencyCal`: This is the calibration
    version of :class:`~qiskit_experiments.library.characterization.RamseyXY`.

  * :class:`~qiskit_experiments.library.characterization.FineFrequency`:
    This experiment performs an error amplifying sequence to measure the frequency of the qubit.
    This is done with delay instructions with a variable length and RZ gates.

  * :class:`~qiskit_experiments.library.calibration.FineFrequencyCal`:
    This is the calibration version of
    :class:`~qiskit_experiments.library.characterization.FineFrequency`.

  * :class:`~qiskit_experiments.library.characterization.HalfAngle`:
    This experiment measures the amount by which the SX and X gates are not parallel.
    Such errors can occur due to phase errors. For example,
    the non-linearities in the mixer's skew for :math:`\pi/2` pulses may be
    different from the :math:`\pi` pulse.

  * :class:`~qiskit_experiments.library.calibration.HalfAngleCal`:
    This is the calibration version of
    :class:`~qiskit_experiments.library.characterization.HalfAngle`.

.. releasenotes/notes/0.2/library-26e6cf3dfbc3acb3.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Two cross-resonance Hamiltonian tomography experiments have been
  added to the :mod:`qiskit_experiments.library`.

  * :class:`qiskit_experiments.library.CrossResonanceHamiltonian`
  * :class:`qiskit_experiments.library.EchoedCrossResonanceHamiltonian`

  These experiments estimates the IX, IY, IZ, ZX, ZY, ZZ Hamiltonian term
  coefficients of the cross-resonance Hamiltonian, using either a single-tone
  cross-resonance gate
  (:class:`~qiskit_experiments.library.CrossResonanceHamiltonian`)
  or an echoed cross-resonance gate
  (:class:`~qiskit_experiments.library.EchoedResonanceHamiltonian`).

.. releasenotes/notes/0.2/library-26e6cf3dfbc3acb3.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Added a :class:`qiskit_experiments.library.ReadoutAngle` characterization
  experiment. This experiment computes the average of the angles of the IQ
  clusters of the ground and excited states.

.. releasenotes/notes/0.2/library-26e6cf3dfbc3acb3.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- :class:`~qiskit_experiments.library.StandardRb` and
  :class:`~qiskit_experiments.library.InterleavedRb` experiments now
  compute error as part of the error-per-gate computation.


.. _Release Notes_0.2.0_Known Issues:

Known Issues
------------

.. releasenotes/notes/0.2/curve-analysis-f4d62e011815c5c3.yaml @ b'140480e060e2fff2c010a4a4bdc2bd0cffa2cd1c'

- Curve analysis may have imperfection in the uncertainty propagation computation.
  Fit paramters consist of the nominal part and standard error, however,
  the correlation of these paramters are not precisely taken into account.
  This sometimes result in the overestimation of the confidence interval of fit curves,
  or overestimation of the standard error of some analysis values computed with
  multiple fitting parameters. This issue will be solved in the version 0.3.
  See `qiskit-experiments/#551 <https://github.com/Qiskit/qiskit-experiments/pull/551>`_
  for details.


.. _Release Notes_0.2.0_Upgrade Notes:

Upgrade Notes
-------------

.. releasenotes/notes/0.2/0_2_release-eef5e3ba256fc750.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- The default ``matplotlib`` backend used for generated figures was changed
  from `AGG` to `SVG`.

.. releasenotes/notes/0.2/0_2_release-eef5e3ba256fc750.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Due to changes in JSON serialization it is possible that some experiments
  saved to the IBM Experiments database service using the Qiskit Experiments
  0.1 API may not be able to be loaded using Qiskit Experiments 0.2.

.. releasenotes/notes/0.2/base-analysis-b261afaa40518b53.yaml @ b'770a3dffd30d9093ec20ad85676f0b2f92393c4a'

- Changed :meth:`~qiskit_experiments.framework.BaseAnalysis.run` to run
  asynchronously using the
  :meth:`~qiskit_experiments.framework.ExperimentData.add_analysis_callback`.
  Previously analysis was only run asynchronously if it was done as part of
  an experiments :meth:`~qiskit_experiments.framework.BaseExperiment.run`.

.. releasenotes/notes/0.2/base-experiment-14eba2646ef0f0b9.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- The :meth:`qiskit_experiments.framework.BaseExperiment.analysis` property
  has been changed to return a :class:`qiskit_experiments.framework.BaseAnalysis`
  *instance* rather than a class type. This method also now has a setter
  which allows setting an analysis instance for use by an experiment.

.. releasenotes/notes/0.2/calibrations-97c6ae807d54015d.yaml @ b'770a3dffd30d9093ec20ad85676f0b2f92393c4a'

- The ``BackendCalibrations`` class has bas been removed and its functionality
  has been merged into the
  :class:`qiskit_experiments.calibration_management.Calibrations` class. Users
  should now use the :class:`Calibrations`
  class which can be instantiated from a backend by using the
  :meth:`~qiskit_experiments.calibration_management.Calibrations.from_backend`
  method.

.. releasenotes/notes/0.2/data-processors-d6430844d2701eb1.yaml @ b'a03eb85832e5b10ff32e71e9725b307fd0f1ada4'

- Data format used in the :class:`qiskit_experiments.data_processing.DataProcessor`
  has been changed from `Tuple[Any, Any]` to `np.ndarray`.

.. releasenotes/notes/0.2/data-processors-d6430844d2701eb1.yaml @ b'a03eb85832e5b10ff32e71e9725b307fd0f1ada4'

- Uncertainty propagation in the
  :class:`qiskit_experiments.data_processing.DataProcessor` class is now
  computed using the `uncertainties <https://pythonhosted.org/uncertainties/>`_
  package.
  See :mod:`qiskit_experiments.data_processing` module documentation for details.

.. releasenotes/notes/0.2/experiment-data-5465208160fe6b6a.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- The ``CompositeExperimentData`` class has been removed and its
  functionality integrated into the
  :class:`~qiskit_experiments.framework.ExperimentData` class.
  A composite :class:`~qiskit_experiments.framework.ExperimentData`
  can now be created by initializing with a list of child
  ``ExperimentData`` containers using the ``child_data`` kwarg.

.. releasenotes/notes/0.2/experiment-data-5465208160fe6b6a.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- :class:`~qiskit_experiments.framework.ParallelExperiment` and
  :class:`~qiskit_experiments.framework.BatchExperiment` now return
  a :class:`~qiskit_experiments.framework.ExperimentData` object
  which no longer contains a ``component_experiment_data`` method.
  This method has been replaced by the
  :meth:`~qiskit_experiments.framework.ExperimentData.child_data`
  method.

.. releasenotes/notes/0.2/experiment-data-5465208160fe6b6a.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- The :meth:`qiskit_experiments.framework.ExperimentData.analysis_results`
  method has been changed to block on analysis callbacks finishing by
  default, this means it is no longer necessary to call the
  :meth:`~qiskit_experiments.framework.ExperimentData.block_for_results`
  method first before accessing analysis results.

  To disable blocking this can be set to run with ``block=False``.
  This should be used
  :meth:`~qiskit_experiments.framework.ExperimentData.analysis_results`
  needs to be called during another analysis callback to prevent that
  callback from blocking.

.. releasenotes/notes/0.2/experiment-data-5465208160fe6b6a.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- The ``callback`` and ``**kwarg`` arguments have been removed from
  :meth:`~qiskit_experiments.framwork.ExperimentData.add_data`
  To add a callback function to run after experiment jobs have finished
  executing use the
  :meth:`~qiskit_experiments.framework.ExperimentData.add_analysis_callback`
  method instead.

.. releasenotes/notes/0.2/library-26e6cf3dfbc3acb3.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- All :mod:`qiskit_experiments.library` experiments have been changed to work
  with fixed SI units: `Hz` for frequency, `seconds` for delays, and backend
  `dt` for pulse widths and durations.
  Previous experiments with ``unit`` kwargs in their init functions have had
  this kwarg removed.

.. releasenotes/notes/0.2/library-26e6cf3dfbc3acb3.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- The ``qubits`` intitialization argument for
  :class:`~qiskit_experiments.library.StandardRb`,
  :class:`~qiskit_experiments.library.InterleavedRb`,
  :class:`~qiskit_experiments.library.QuantumVolume`,
  :class:`~qiskit_experiments.library.StateTomography`,
  and
  :class:`~qiskit_experiments.library.ProcessTomography`
  no longer accepts interger values for specifying a range of qubits.
  and  must now contain an explicit sequence of qubits.

.. releasenotes/notes/0.2/library-26e6cf3dfbc3acb3.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- The behavior of the ``seed`` initialization kwarg of the
  :class:`~qiskit_experiments.library.StandardRB`,
  :class:`~qiskit_experiments.library.InterleavedStandardRB`,
  :class:`~qiskit_experiments.library.QuantumVolume` experiments has
  been modified.

  In the new version the ``seed`` value is used as to initialize a Numpy
  random number generator object as ``numpy.random.default_rng(seed=seed)``
  each time the experiments ``circuits`` method is called. This change means
  that using a fixed seed value will result in the same circuits being
  generated each time an experiment is run, if no other
  experiment options are changed.

  To generate different new random circuits each time an experiment is run
  the (default) value of ``seed=None`` should be used. To reproduce
  equivalent functionality to the previous version behavior of differnet
  circuits being generated each time ``run`` is called with a fixed seed
  you must now set a new fixed seed value between each call to ``run``
  using ``experiment.set_experiment_options(seed=value)``.


.. _Release Notes_0.2.0_Deprecation Notes:

Deprecation Notes
-----------------

.. releasenotes/notes/0.2/base-experiment-14eba2646ef0f0b9.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- The :meth:`qiskit_experiments.framework.BaseExperiment.set_analysis_options`
  method has been deprecated, use the
  :meth:`qiskit_experiments.framework.BaseAnalysis.set_options` method
  for the experiments analysis class instead. This can be accessed from the
  experiment instance using the
  :meth:`qiskit_experiments.framework.BaseExperiment.analysis` property as
  ``experiment.analysis.set_options(**options)``.

.. releasenotes/notes/0.2/base-experiment-14eba2646ef0f0b9.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- The :meth:`qiskit_experiments.framework.BaseExperiment.analysis_options`
  property has been deprecated, use the
  :meth:`qiskit_experiments.framework.BaseAnalysis.options` property
  for the experiments analysis class instead. This can be accessed from the
  experiment instance using the
  :meth:`qiskit_experiments.framework.BaseExperiment.analysis` property as
  ``experiment.analysis.options``.

.. releasenotes/notes/0.2/base-experiment-14eba2646ef0f0b9.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- The :meth:`qiskit_experiments.framework.BaseExperiment.run_analysis` and
  method has been deprecated, use the
  :meth:`qiskit_experiments.framework.BaseAnalysis.run` method
  for the experiments analysis class instead. This can be accessed from the
  experiment instance using the
  :meth:`qiskit_experiments.framework.BaseExperiment.analysis` property as
  ``experiment.analysis.run(**kwargs)``.

.. releasenotes/notes/0.2/base-experiment-14eba2646ef0f0b9.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Boolean values for the ``analysis`` kwarg in
  :meth:`qiskit_experiments.framework.BaseExperiment.run` have been deprecated.
  Use ``analysis="default"`` instead of ``analysis=True``, and
  ``analysis=None`` instead of ``analysis=False``.

.. releasenotes/notes/0.2/base-experiment-14eba2646ef0f0b9.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Using the ``__analysis_class__`` class attrbiute to define a default
  :class:`~qiskit_experiments.framework.BaseAnalysis` class in a
  :class:`~qiskit_experiments.framework.BaseExperiment` subclass has
  been deprecated. A default analysis class instance should now be passed to
  the initialization method of
  :class:`~qiskit_experiments.framework.BaseExperiment` instead.


.. _Release Notes_0.2.0_Bug Fixes:

Bug Fixes
---------

.. releasenotes/notes/0.2/data-processors-d6430844d2701eb1.yaml @ b'a03eb85832e5b10ff32e71e9725b307fd0f1ada4'

- The :class:`qiskit_experiments.data_processing.Probability` data processing
  node will no longer return exactly 0 or 1 for a probability estimate. This
  fixes an issue where this could cause division by 0 when computing weights
  during curve fitting analysis.

.. releasenotes/notes/0.2/experiment-data-5465208160fe6b6a.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Fixed a bug where the
  :meth:`qiskit_experiments.framework.ExperimentData.load` method
  would return an
  :class:`~qiskit_experiments.database_service.DbExperimentDataV1` object
  instead of a :class:`~qiskit_experiments.framework.ExperimentData` object.

.. releasenotes/notes/0.2/experiment-data-5465208160fe6b6a.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Fixed bug in :class:`qiskit_experiments.framework.ExperimentData` where
  trying to load saved job data from a backend using
  ``ExperimentData(backend=backend, job_ids=job_ids)`` resulted in an error.

.. releasenotes/notes/0.2/library-26e6cf3dfbc3acb3.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Fixed a bug in :class:`~qiskit_experiments.library.StandardRB` and
  :class:`~qiskit_experiments.library.InterleavedRB` where the variance in
  the estimated error per Clifford did not scale correctly with the number
  of sampled RB sequences.

  See `Issue 428 <https://github.com/Qiskit/qiskit-experiments/issues/428>`_
  for details.


.. _Release Notes_0.2.0_API Changes for Experiment Authors:

API Changes for Experiment Authors
----------------------------------

.. releasenotes/notes/0.2/0_2_release-eef5e3ba256fc750.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Added hooks for serialization and deserialization of custom classes to
  the JSON :class:`qiskit_experiments.framework.ExperimentEncoder`
  and :class:`qiskit_experiments.framework.ExperimentDecoder`.
  To enable serialization classes must implement a method
  ``__json_encode__(self) -> Any`` that returns a JSON serializable object,
  and a class method ``_json_decode__(cls, value: Any)__ -> cls`` that can
  reconstruct the object from the JSON deserialized value.

.. releasenotes/notes/0.2/base-analysis-b261afaa40518b53.yaml @ b'770a3dffd30d9093ec20ad85676f0b2f92393c4a'

- The :class:`qiskit_experiments.framework.BaseAnalysis` class has
  been changed to be an initialized class.

  This class now stores its set analysis options using the
  :meth:`~qiskit_experiments.framework.BaseAnalysis.set_options` and
  :meth:`~qiskit_experiments.framework.BaseAnalysis.options` and
  ``_default_options`` methods.
  The signature of the abstract method ``_run_analysis`` that must be
  implemented by subclasses has been changed to remove the ``**kwargs``.

  Note that the state of this class should only be used to store option
  values and derived configuration. The
  :meth:`~qiskit_experiments.framework.BaseAnalysis.run` and
  ``_run_analysis`` methods should not change the state of the instance.

.. releasenotes/notes/0.2/base-experiment-14eba2646ef0f0b9.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- The :class:`qiskit_experiments.framework.BaseExperiment` class has
  been changed to optionally store an instance of a
  :class:`qiskit_experiments.framework.BaseAnalysis` class  during
  its initialization. Any default analysis options specific to a
  particular experiment subclass should be set during that experiments
  init method, or as default options of the analysis subclass used by
  that experiment.

.. releasenotes/notes/0.2/base-experiment-14eba2646ef0f0b9.yaml @ b'd04d99e73a6eee1af570cecc45bab8a3e8abc687'

- Added a ``_set_backend`` method to
  :class:`~qiskit_experiments.framework.BaseExperiment` that is called
  when a backend is set via initalization or the ``backend`` setter. This
  can be overridden in experiment subclasses if required. For example this
  could be used to extract any needed configuration or properties from the
  specified backend, or to update experiment options of configuration based
  on the backend.

.. releasenotes/notes/0.2/calibrations-97c6ae807d54015d.yaml @ b'770a3dffd30d9093ec20ad85676f0b2f92393c4a'

- :mod:`~qiskit_experiments.library.calibration` experiments have been changed
  so that each experiment is a subclass of a
  :mod:`~qiskit_experiments.library.characterization` experiment.

  Calibration experiments should now be constructed as subclasses of both
  the relevant characterization experiment and the new
  :class:`qiskit_experiments.calibration_management.BaseCalibrationExperiment`
  abstract base class.

  This new base class implements the functionality to manage schedules
  stored in instances of the
  :class:`qiskit_experiments.calibration_management.Calibrations` class and
  implements a framework where calibration experiments can specify an
  :meth:`~qiskit_experiments.calibration_management.BaseCalibrationExperiment.update`
  method to update the parameters that they are designed to calibrate.

.. releasenotes/notes/0.2/curve-analysis-f4d62e011815c5c3.yaml @ b'140480e060e2fff2c010a4a4bdc2bd0cffa2cd1c'

- There have been several changes to the
  :class:`qiskit_experiments.curve_analysis.CurveAnalysis` class which may
  effect developers implementing experiments using curve analysis.

  These include:

  * Default data pre-processing for curve fit data has been added to
    :class:`~qiskit_experiments.curve_analysis.CurveAnalysis`. The input data
    to the analysis is averaged over the same x values and sorted by x values.
    This processing is necessary to correctly analyze an experimental data which
    has duplicate x values.

  * The handling of user provided initial guesses and fit boundaries has been
    changed so that subclasses no longer need to use user provided option values.
    Propagation of user values is now handled by the base class.

  * The subclass method name to provide these initial guesses for curve fitting
    has bee changed from ``_setup_fitting`` to
    :meth:`~qiskit_experiments.curve_analysis.CurveAnalysis._generate_fit_guesses`.
    See the documentation for more details.

  * Tick labels and fit parameters shown in the fit result plot generated by
    :class:`qiskit_experiments.curve_analysis.CurveAnalysis`
    subclasses are formatted with unit with prefactor scalings.
    For example, 1e7 can now be displayed as 10 MHz.

    To enable this change, all experiments using
    :class:`~qiskit_experiments.curve_analysis.CurveAnalysis` or a subclass
    analysis class should set analysis options ``xval_unit`` and ``yval_unit``.

.. releasenotes/notes/0.2/curve-analysis-f4d62e011815c5c3.yaml @ b'140480e060e2fff2c010a4a4bdc2bd0cffa2cd1c'

- A ``model_description`` fields has been added to the
  :class:`qiskit_experiments.curve_analysis.SeriesDef` dataclass. This field
  stores the string representation of the fit model of the curve and will be
  stored in the experiments analysis result extra field.
